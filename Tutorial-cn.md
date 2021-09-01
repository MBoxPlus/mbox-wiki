**简体中文** | [English](https://github.com/MBoxPlus/mbox/wiki/Tutorial) 
## 创建 Workspace

使用命令 `mbox init [PLUGIN_GROUP]` 来创建一个 workspace。 例如，运行 `mbox init ios` 将会使得当前目录成为一个 workspace。

```shell
$ mbox init ios
Create MBox configuration file
Init mbox workspace success.
Enable Plugin MBoxCocoapods 1.2.4
Enable Plugin MBoxDependencyManager 1.4.3
Enable Plugin MBoxContainer 1.0.4
Enable Plugin MBoxGit 1.2.3
Enable Plugin MBoxRuby 1.1.2
Enable Plugin MBoxWorkspace 1.2.7
Enable Plugin MBoxCore 2.2.4
Plugin new: 7, update: 0, delete: 0
```

使用命令 `mbox status` 将会打印当前 workspace 的状态信息。

```shell
$ demo mbox status
[Root]: /Users/mbox/demo

[Feature]: FreeMode

    It is empty!

[Containers]:
    It is empty!
```

`mbox --help` 或者 `mbox [SUB_COMMAND] --help` 将输出简单的使用文档。

```shell
$ mbox --help
Usage:

    $ mbox

Commands:

    + activate      Activate a or more components
    + add           Add a repo into current feature
    + bundle        Redirect to Bundler with MBox environment
    + config        Get/Set Default Configuration
    + container     Manage Container
    + deactivate    Deactivate a or more components
    + depend        Show/Change dependencies
    + env           Show MBox Environment
    + exec          Exec command line in MBox Environment
    + feature       Manage Features
    + gem           Redirect to Gem with MBox environment
    + git           Execute git command for every repo
    + git-sheet     Execute git command for every repo and format output
    + go            Quckly open workspace.
    + merge         Merge `other feature`/`target branch` into current feature
    + new           Create a project in workspace
    + open          Open specific path in MBox Environment
    + plugin        Manage Plugins
    + pod           Redirect to Bundler with MBox environment
    + remove        Remove project from workspace
    + repo          Manage Repos
    + setup         Setup Command Line Tool
    + status        Show Status

Options:

    --api     Output information with API format. Avaliable: json/none/plain/yaml
    --root    Set Root Directory
    --home    Set Configuration Home Directory

Flags:

        --launcher    Check Launcher environment, defaults to True
    -v, --verbose     Output verbose log
    -h, --help        Show help information
        --version     Show version
```

## 管理 Repo

### 添加 Repo

可以使用 `mbox add` 把一个仓库添加到当前的 feature 中来。

```shell
$ mbox add https://github.com/MBoxPlus/MBoxReposDemo.git main
Prepare Repository
Setup Remote Repository
  Clone from `https://github.com/MBoxPlus/MBoxReposDemo.git`
    Cloning into 'Cx4qeW'...
    remote: Enumerating objects: 262, done.
    remote: Counting objects: 100% (262/262), done.
    remote: Compressing objects: 100% (144/144), done.
    remote: Total 262 (delta 118), reused 252 (delta 108), pack-reused 0
    Receiving objects: 100% (262/262), 201.99 KiB | 587.00 KiB/s, done.
    Resolving deltas: 100% (118/118), done.
Setup Git Reference
  Setup repo `MBoxReposDemo`, based branch `main`
Add `MBoxReposDemo` into feature `FreeMode`
Preprocess Origin Repository
Setup Work Repository
Checkout branch `main`
Add repo `MBoxReposDemo` success.
Remove Local Dependency
Show Status
  [Root]: /Users/mbox/demo

  [Feature]: FreeMode

      MBoxReposDemo  https://github.com/MBoxPlus/MBoxReposDemo.git  [main]   ↑0  ↓0
        + [CocoaPods]  MBoxReposDemo

  [Containers]:
   => MBoxReposDemo  Bundler + CocoaPods
```

在 [free mode](https://github.com/MBoxPlus/mbox/wiki/MBox-terminology#free-mode-%E5%92%8C-feature-mode) 下, 该命令有 **2** 个参数，第1个是 git URL 地址，第2个是即将要切出的 git 分支或标签名。

在 [feature mode](https://github.com/MBoxPlus/mbox/wiki/MBox-terminology#free-mode-%E5%92%8C-feature-mode)下, 该命令有 **3** 个参数，。 第1个是 git URL 地址。第2个是当前即将切出的 feature 分支的基线（可以是分支/标签/提交）。第三个参数是目标分支，即最终 MR 期望合入的分支。

### 删除 Repo

可使用 `mbox remove` 来把一个仓库从当前 feature 中删除。

```shell
$ mbox status
[Root]: /Users/mbox/demo

[Feature]: test

    MBoxReposDemo  https://github.com/MBoxPlus/MBoxReposDemo.git  [feature/test]   ->  [main]     ↳0  ↰0
      + [CocoaPods]  MBoxReposDemo
    SnapKit        https://github.com/SnapKit/SnapKit.git         [feature/test]   ->  [develop]  ↳0  ↰0
      + [CocoaPods]  SnapKit

[Containers]:
 => MBoxReposDemo  Bundler + CocoaPods
 
 # 从上面 status 中可以看到 SnapKit，下面我们使用 remove 命令将其从当前 feature 移除
$ mbox remove SnapKit
Validate Repo
Remove Repo
Show Status
  [Root]: /Users/mbox/demo

  [Feature]: test

      MBoxReposDemo  https://github.com/MBoxPlus/MBoxReposDemo.git  [feature/test]   ->  [main]  ↳0  ↰0
        + [CocoaPods]  MBoxReposDemo

  [Containers]:
   => MBoxReposDemo  Bundler + CocoaPods
```

我们从 `test` feature 中删除了一个名为 `SnapKit` 的仓库。所有基于 repo 的操作带来的影响仅限于当前 feature 内部。

### Git

#### mbox git

`mbox git` 通过批处理命令来帮助管理多个仓库。 `mbox git` 是基于 [libgit2](https://github.com/libgit2/libgit2) 开发的，目的并不是要取代 Git，你仍然可以使用原生 `git` 命令来管理仓库。 


```shell
# Pull 所有仓库代码
$ mbox git pull
[mbox-core]
  Already up to date.
[mbox-git]
  Already up to date.

# Fetch 所有仓库
$ mbox git fetch

# 切换所有仓库到 master 分支
$ mbox git checkout master
[mbox-core]
  Updating files: 100% (6214/6214), done.
  Switched to branch 'master'
  Your branch is up to date with 'origin/master'.
[mbox-git]
  Switched to branch 'master'
  Your branch is up to date with 'origin/master'.

# 取消所有仓库的本地修改
❯ mbox git checkout .
[mbox-core]
  Updated 3 paths from the index
[mbox-git]
  Updated 0 paths from the index
```

#### mbox git-sheet

```shell
mbox git-sheet
```

和之前的 `mbox git` 命令类似，优化了日志输出，不再输出每个仓库的 git 信息，使用报表的形式输出结果。

目前支持以下命令：

- fetch
- pull
- push
- status

例子：

```shell
$ mbox git-sheet status
    mbox-core                [master]   ↑0  ↓0
    mbox-ruby                [master]   ↑0  ↓0
    mbox-container           [master]*  ↑0  ↓0
```

#### mbox merge

`mbox merge` 是另一个与 Git 操作相关的命令，但它仅能在 feature mode 中使用。这个命令能帮助你快速地将目标分支的代码同步到你当前的 feature 分支中来。

```shell
$ mbox merge
[MBoxReposDemo]
  Merge branch `origin/main` into branch `feature/test_git`.
  There is nothing to merge.
[MBoxReposDemo2]
  Merge branch `origin/main` into branch `feature/test_git`.
  There is nothing to merge.
[SnapKit]
  Merge branch `origin/develop` into branch `feature/test_git`.
  There is nothing to merge.
```

## 管理 Features

### 查看 Features 列表

我们可以使用 `mbox feature list` 来列出所有的 feature。

```shell
# 展示所有 feature
$ mbox feature list
[bugfix]
  MBoxReposDemo
[FreeMode]
  MBoxReposDemo
  SnapKit
[test]
  MBoxReposDemo
```
在这里可以看到有2个feature（分别名为 'bugfix' 和 'test'）和1个 free-mode 在这个 workspace 中，free-mode 可以认为是一个默认的特殊 feature。

### 创建 Feature

我们可以基于 free-mode 或另一 feature 来创建一个新 feture。前一个 feature 的仓库列表将被拷贝到新的 feature 中，但是上述的两种情况会有些许差别。

如果基于 free-mode，free-mode 下所有仓库当前的 HEAD 指针指向的节点将会作为新  feature 的基线被切出。如果是基于另一个 feature 来创建新 feature，那么当前 feature 所有仓库的目标分支将作为新 feature 的基线被切出。同时，所有未提交的改动也将会被保存起来，不会带入到新 feature 中。

```shell
# 当前是 FreeMode
$ mbox status
[Root]: /Users/mbox/demo

[Feature]: test

    MBoxReposDemo  https://github.com/MBoxPlus/MBoxReposDemo.git  [feature/test]   ->  [main]     ↳0  ↰0
      + [CocoaPods]  MBoxReposDemo
    SnapKit        https://github.com/SnapKit/SnapKit.git         [feature/test]   ->  [develop]  ↳0  ↰0
      + [CocoaPods]  SnapKit

[Containers]:
 => MBoxReposDemo  Bundler + CocoaPods

# 从 FreeMode 中切出一个名为 test 的新 feature 
$ mbox feature start test
Create a new feature `test`
Save current git HEAD
Stash previous feature `FreeMode`
Backup support files for feature `FreeMode` (Mode: Keep)
Check repo exists
Update workspace
Checkout feature `test`
Pick stash from `FreeMode` into new feature `test`
Show Status
  [Root]: /Users/bytedance/liyao/demo

  [Feature]: test

      MBoxReposDemo  https://github.com/MBoxPlus/MBoxReposDemo.git  [feature/test]   ->  [main]     ↳0  ↰0
        + [CocoaPods]  MBoxReposDemo
      SnapKit        https://github.com/SnapKit/SnapKit.git         [feature/test]   ->  [develop]  ↳0  ↰0
        + [CocoaPods]  SnapKit

  [Containers]:
   => MBoxReposDemo  Bundler + CocoaPods

[!] The sandbox is not in sync with the Podfile.lock. You may need to run 'mbox pod install'.
```

### 删除 Feature

使用命令 `mbox feature remove` 可以删除一个 feature。

## Multiple Containers

多个 Container 同时存在在同一个 feature 中是被允许的，我们可以使用 `mbox container use/disuse` 来切换容器。

```shell
# 当前容器是 MBoxReposDemo
$ mbox status
[Root]: /Users/mbox/demo

[Feature]: FreeMode

    SnapKit         https://github.com/SnapKit/SnapKit.git          [develop]   ↑0  ↓0
      + [CocoaPods]  SnapKit
    MBoxReposDemo   https://github.com/MBoxPlus/MBoxReposDemo.git   [main]      ↑0  ↓0
      + [CocoaPods]  MBoxReposDemo
    MBoxReposDemo2  https://github.com/MBoxTest/MBoxReposDemo2.git  [main]      ↑0  ↓0
      + [CocoaPods]  MBoxReposDemo2

[Containers]:
 => MBoxReposDemo   Bundler + CocoaPods
    MBoxReposDemo2  Bundler + CocoaPods
```

```shell
# 切换到 MBoxReposDemo2
$ mbox container use MBoxReposDemo2
Use container `MBoxReposDemo2` for CocoaPods
Use container `MBoxReposDemo2` for Bundler
Show Status
  [Root]: /Users/mbox/demo

  [Feature]: FreeMode

      SnapKit         https://github.com/SnapKit/SnapKit.git          [develop]   ↑0  ↓0
        + [CocoaPods]  SnapKit
      MBoxReposDemo   https://github.com/MBoxPlus/MBoxReposDemo.git   [main]      ↑0  ↓0
        + [CocoaPods]  MBoxReposDemo
      MBoxReposDemo2  https://github.com/MBoxTest/MBoxReposDemo2.git  [main]      ↑0  ↓0
        + [CocoaPods]  MBoxReposDemo2

  [Containers]:
      MBoxReposDemo   Bundler + CocoaPods
   => MBoxReposDemo2  Bundler + CocoaPods
```

这个 feature 当前包含两个 container 了 `MBoxReposDemo` 和 `MBoxReposDemo2`，我们从 `MBoxReposDemo` 切换到了 `MBoxReposDemo2`。当下次执行 `mbox pod install` 时，MBox 将会重新组装项目 `MBoxReposDemo2`。
 
在运行项目集成命令之前，比如 `mbox pod install` 或者 `mbox bundle install`，我们必须使用 `mbox container use` 命令来选择一个 container。

## MBox 与 依赖管理

MBox 为了简化研发人员对于依赖组件的管理，抽象了组件的概念，将仓库与组件进行了关联管理，从而可以达到更好的体验效果：

1. `mbox add`/`mbox remove` 命令可以通过直接传递组件的 `NAME` 而直接下载仓库，甚至可以从主容器的依赖图中分析出当前该组件所使用的版本号，从而切换到对应的 Commit 节点，全程无需用户输入，
1. `mbox pod install`/`mbox bundle install` 等安装依赖的命令，通过分析 Workspace 内仓库的组件信息，自动使用本地仓库的组件，直接进入开发模式，不再需要用户修改任何文件

### 依赖管理工具

MBox 通过插件化，可以不断增加依赖管理工具：

1. `Bunder`: 由插件 `MBoxRuby` 提供依赖管理功能
1. `CocoaPods`: 由插件 `MBoxCocoapods` 提供依赖管理功能

后续还会不断开放更多的依赖管理工具，包括 `Gradle`、`Flutter` 等。

依赖管理功能分为两个方面：容器和组件。

#### 容器

容器指的是拥有完整的依赖关系，能够独立运行的 App。容器的判断是由依赖管理工具提供的，不同的依赖管理工具，能够识别不同的容器。

1. `Bundler` 容器指的是拥有 `Gemfile` 的项目，具体配置方式详见 [MBoxRuby 插件](https://github.com/MBoxPlus/mbox-ruby)
1. `CocoaPods` 容器指的是拥有 `Podfile` 的项目，具体配置方式详见 [MBoxCocoapods 插件](https://github.com/MBoxPlus/mbox-cocoapods)

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

#### 组件

与容器对应的，MBox 不仅要能识别容器，还要能识别对应的组件，提供给容器进行安装等操作。

1. `Bundler` 组件指的是拥有 `gemspec` 的项目
1. `CocoaPods` 组件指的是拥有 `podspec` 的项目

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

### 单仓多组件

一般一个仓库开发一个组件，以 iOS 为例，仓库内有一个 `.podspec` 文件来描述该组件的信息。

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

### 多平台组件

有些项目是跨端项目，仓库内会同时存在多端的组件，MBox 通过激活相关插件后，能够同时显示所有组件，例如在 `mbox status` 时显示：

```shell
$ mbox status
  mbox-container  git@github.com:mboxplus/mbox-container.git   [master]*  ↑0  ↓0
    - [Bundler]    mbox-container
    + [CocoaPods]  MBoxContainer
```

此时仓库下有两种组件：

1. `Bundler` 组件，组件名为 `mbox-container`，未激活该组件
2. `CocoaPods` 组件，组件名为 `MBoxContainer`，激活该组件

无论是 `单仓多组件` 和 `多平台组件`，都可以通过 `mbox active/deactive` 命令控制组件的激活状态。

### 开发自己的依赖管理工具

与开发标准插件类似，依赖 `MBoxContainer` 和 `MBoxDependencyManager`，需要 Hook 三个方法，以 `MBoxRuby` 添加 `Bundler` 依赖为例：

```swift
// 添加依赖管理工具 Bundler
extension MBDependencyTool {
    public static let Bundler = MBDependencyTool("Bundler")

    @_dynamicReplacement(for: allTools)
    public static var ruby_allTools: [MBDependencyTool] {
        var tools = self.allTools
        tools.insert(.Bundler, at: 0)
        return tools
    }
}


extension MBWorkRepo {
    // 添加对 Bundler 容器的识别
    @_dynamicReplacement(for: fetchContainers())
    open func ruby_fetchContainers() -> [MBContainer] {
        var value = self.fetchContainers()
        if self.path.appending(pathComponent: "Gemfile").isExists {
            value.append(MBContainer(name: self.name, tool: .Bundler))
        }
        return value
    }

    // 添加对 Bundler 组件的识别
    @_dynamicReplacement(for: resolveDependencyNames())
    open func ruby_resolveDependencyNames() -> [(tool: MBDependencyTool, name: String)] {
        var names = self.resolveDependencyNames()
        // allGemspecPaths 方法获取仓库下所有 gemspec 文件
        let data = self.allGemspecPaths().map {
            (
                tool: MBDependencyTool.Bundler,
                name: $0.lastPathComponent.fileName
            )
        }
        names.append(contentsOf: data)
        return names
    }
}
```
