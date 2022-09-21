# MBox 与 Dependency

当 Workspace 里有多个仓库的时候，每个仓库可能扮演不同角色。例如，有一个仓库是主工程，其他仓库是 SDK。

MBox 将主工程称之为 Container，SDK 工程称之为 Component。

> `Container` 指的是拥有完整的依赖关系，能够独立运行的 App。`Container` 的判断是由依赖管理工具提供的，不同的依赖管理工具，能够识别不同的 `Container`。

> `Component` 指的是可以作为依赖被其他项目所引入。`Component` 的判断是由依赖管理工具提供的，不同的依赖管理工具，能够识别不同的 `Component`。

一个仓库里面可能既包含主工程，也包含 SDK，所以，这种仓库既是 Container，又是 Component。

一个仓库可能包含多个 Container（我们称之为 `单仓多容器`），也可能包含多个 Component（我们称之为 `单仓多组件`）。

所以，一个 Workspace 内，就可能存在多个 Container 和 多个 Component。

另外，Container 和 Component 都是有相对环境的。例如，一个仓库里面有 iOS 的主工程，也有 Android 主工程，那么这个仓库就有两个 Container，分别是 iOS 和 Android 两个依赖管理工具判断的。同样的，组件也是区分依赖管理工具的。

## 体验优化

在日常开发中，遇到最多的就是依赖的版本控制，包括将依赖下载到本地进行开发。MBox 在依赖管理方面做了很多工作，抽象了依赖管理工具 `MBoxDependencyManager`，进一步抽象出了 容器 和 组件 两个概念，从而可以达到更好的体验效果：

1. `mbox add`/`mbox remove` 命令可以通过直接传递组件的 `NAME` 而直接下载仓库，甚至可以从主容器的依赖图中分析出当前该组件所使用的版本号，从而切换到对应的 Commit 节点，全程无需用户输入，
1. `mbox pod install`/`mbox bundle install` 等安装依赖的命令，通过分析 Workspace 内仓库的组件信息，自动使用本地仓库的组件，直接进入开发模式，不再需要用户修改任何文件

## 依赖管理工具

`MBoxDependencyManager` 只是一个抽象的依赖管理工具，通过插件化，不断增加具体的依赖管理工具：

1. [MBoxRuby 插件](https://github.com/MBoxPlus/mbox-ruby): 
    - 通过识别 `Gemfile` 来判断 `Bundler` 容器
    - 通过识别 `.gemspec` 来判断 `Bundler` 组件
1. [MBoxCocoapods 插件](https://github.com/MBoxPlus/mbox-cocoapods):
    - 通过识别 `Podfile` 来判断 `CocoaPods` 容器
    - 通过识别 `.podspec` 来判断 `CocoaPods` 组件
1. [MBoxDart 插件](https://github.com/MBoxPlus/mbox-dart) 用于 Flutter 依赖分析:
    - 通过识别 `pubspec.lock` 来判断 `Dart` 容器
    - 通过识别 `pubspec.yml` 来判断 `Dart` 组件

后续还会不断开放更多的依赖管理工具，包括 `Gradle` 等。

依赖管理功能分为两个方面：容器和组件。

### 容器

> 容器指的是拥有完整的依赖关系，能够独立运行的 App。容器的判断是由依赖管理工具提供的，不同的依赖管理工具，能够识别不同的容器。

- 当 Workspace 内同平台只有一个 Container 时，默认激活该 Container；
- 当 Workspace 内有多个同平台的 Container 时，可以使用 `mbox container use/disuse` 来切换容器。

#### 多容器切换

当 Workspace 内存在多个容器时，特别是同一个平台的多个容器时，往往需要切换当前容器，从而安装不同的依赖，开发不同的项目。

例如，研发 SDK 的同学，当 SDK 同时集成到 A 和 B 两个项目时，需要分别测试他们，就可以把 SDK、A、B 三个仓库都添加的 Workspace 内：

```shell
$ mbox status
Feature: FreeMode
   MySDK  git@xxx.com:xx/xx.git   [master]  ↑0  ↓0
      + [CocoaPods]  MySDK
   A      git@xxx.com:xx/xx.git   [master]  ↑0  ↓0
   B      git@xxx.com:xx/xx.git   [master]  ↑0  ↓0

Container:
   A     CocoaPods
   B     CocoaPods
```

由于 Workspace 内有 A、B 两个容器，在执行 `mbox pod install` 安装依赖的时候，极易产生依赖冲突。MBox 可以通过切换主容器的方式，让用户能够调试不同的项目来测试自己的 SDK：

```shell
$ mbox container use A
Feature: FreeMode
   MySDK  git@xxx.com:xx/xx.git   [master]  ↑0  ↓0
      + [CocoaPods]  MySDK
   A      git@xxx.com:xx/xx.git   [master]  ↑0  ↓0
   B      git@xxx.com:xx/xx.git   [master]  ↑0  ↓0

Container:
=> A     CocoaPods
   B     CocoaPods

$ mbox pod install
```

上面的例子，通过 `mbox container use A` 激活了主容器 A，然后使用 `mbox pod install` 安装 A 容器的依赖，此时 A 项目能正常运行，且使用了本地的 MySDK 代码。

当测试完成 A 项目，再使用 `mbox container use B` 激活主容器 B，使用 `mbox pod install` 安装 B 容器的依赖，进而测试 MySDK 在 B 项目的效果。

#### 多容器同时激活

MBox 为了方便用户使用，默认关闭了 `相同依赖管理工具下同时激活多容器` 的功能，激活其中一个，会自动取消激活另外一个，以保证同一时间只会有一个 `CocoaPods` 容器被激活。（也就是 `切换容器`）

可以使用配置命令 `mbox config container.allow_multiple_containers Bundler Cocoapods` 来开启指定工具能够同时激活多容器。

启用多容器后，激活一个容器后就不再取消之前的激活，而是实现同时激活的效果：

```shell
$ mbox container use A
Feature: FreeMode
   MySDK  git@xxx.com:xx/xx.git   [master]  ↑0  ↓0
      + [CocoaPods]  MySDK
   A      git@xxx.com:xx/xx.git   [master]  ↑0  ↓0
   B      git@xxx.com:xx/xx.git   [master]  ↑0  ↓0

Container:
=> A     CocoaPods
=> B     CocoaPods
```

当两个容器同时激活，两个容器的 Podfile 会合并成一个，会进行统一版本仲裁，可能会出现大量版本冲突，因此多容器同时激活需要谨慎使用！

该功能适合比较特殊的场景，例如 A 是大型项目，B 是一个小型 Demo，B 里面的依赖没有版本要求，这样就能直接使用 A 中的依赖。

### 组件

> 能作为独立库被其他项目依赖的模块，就称为 组件，例如 iOS 的一个 Pod。组件也会因为不同的依赖管理工具而被识别。

一般来说，添加到本地的仓库，默认都会使用该组件。但是也存在一些情况，并不想使用本地的组件。MBox 提供了 `mbox active/deactive` 命令控制组件的激活状态。

```shell
$ mbox status
Feature: FreeMode
   MySDK  git@xxx.com:xx/xx.git   [master]  ↑0  ↓0
      + [CocoaPods]  MySDK
   A      git@xxx.com:xx/xx.git   [master]  ↑0  ↓0

Container:
=> A    CocoaPods

$ mbox pod install # 此时会使用本地的 MySDK 项目

$ mbox deactive MySDK
Feature: FreeMode
   MySDK  git@xxx.com:xx/xx.git   [master]  ↑0  ↓0
      -  [CocoaPods]  MySDK
   A      git@xxx.com:xx/xx.git   [master]  ↑0  ↓0

Container:
=> A     CocoaPods

$ mbox pod install # 此时不会使用本地的 MySDK 项目，会下载远端的线上 MySDK
```

### 单仓多容器/组件

一般一个仓库开发一个容器/组件，以 iOS 为例，仓库内有一个 `.podspec` 文件来描述该组件的信息，对于容器，有一个 `Podfile` 文件来描述主项目的信息。

例如 `SDWebImage` 项目的根目录有一个 `SDWebImage.podspec` 描述文件描述这个组件信息。

这种一个仓库内只包含一个组件的形态，我们称之为 `单仓单组件`。

与之对应的，当一个仓库内存在多个组件描述文件时，我们称之为 `单仓多组件`。

例如 `mbox-test` 项目根目录结构如下：

```
mbox-test
  |- Podfile
  |- Podfile.lock
  |- mbox-test1.podspec
  |- mbox-test2.podspec
```

`mbox-test` 仓库下有两个组件描述文件：`MBoxTest1.podspec` 和 `MBoxTest2.podspec`，因此 MBox 会同时识别这两个组件，在 `mbox status` 时显示仓库的组件列表：

```shell
$ mbox status
  mbox-test  git@github.com:mboxplus/mbox-test.git   [master]*  ↑0  ↓0
    + [CocoaPods]  MBoxTest1
    + [CocoaPods]  MBoxTest2
```

### 多平台容器/组件

有些项目是跨端项目，仓库内会同时存在多端的容器/组件，MBox 通过激活相关插件后，能够同时显示所有组件，例如在 `mbox status` 时显示：

```shell
$ mbox status
[Feature] FreeMode
  mbox-container  git@github.com:mboxplus/mbox-container.git   [master]*  ↑0  ↓0
    - [Bundler]    mbox-container
    + [CocoaPods]  MBoxContainer
[Container]
   => MBoxContainer CocoaPods
```

此时仓库下有两种组件：

1. `Bundler` 组件，组件名为 `mbox-container`，未激活该组件
2. `CocoaPods` 组件，组件名为 `MBoxContainer`，激活该组件

只有一种容器: `CocoaPods` 容器，容器名为 `MBoxContainer`。

激活/取消激活 组件通过 `mbox active/deactive` 命令控制组件的激活状态。

激活/取消激活 容器通过 `mbox container use/disuse` 命令控制容器的激活状态。

组件/容器 激活命令可以指明 `--tool=Cocoapods` 声明要控制的平台。

