# Workspace 功能概述

MBox 为了更好的管理代码仓库，引入了 Workspace 概念。

Workspace 不仅仅管理了所有仓库，同时也隔离了开发环境。不同的 Workspace 拥有不同的仓库列表、分支信息、开发环境， 甚至也包括不同的 MBox 插件环境。

有几点必须强调：

1. Workspace 是一个目录：因此可以将任意目录作为一个 Workspace。

1. Workspace 不能嵌套：在初始化 Workspace 的时候，会递归向上级目录检查是否是 Workspace，如果是则会创建失败。但是如果是在一个已有的 Workspace 的上层目录初始化 Workspace，由于向下递归搜索工作量太大，因此无法检测，是能初始化成功的，但是后续逻辑可能造成混乱。

1. 用户的 Home 目录及更上层目录是不允许作为 Workspace 的，否则由于 Workspace 不允许嵌套的规则，将会导致整个系统只有一个 Workspace，无法再创建其他的 Workspace！

1. MBox 的 Workspace 相关命令可以在 Workspace 下的任意子目录下运行，不仅仅是 Workspace 的根目录，运行效果都一样。

## 创建 Workspace

我们需要先有一个空目录，把这个目录初始化为 Workspace：

```shell
$ mkdir demo
$ cd demo
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

`mbox init [PLUGIN_GROUP]` 命令将会在当前目录部署 MBox 基础文件。`PLUGIN_GROUP` 参数可以快速激活一组插件，例如，`ios` 包括 `MBoxCocoapods`等。使用命令 `mbox init --help` 可以查看有效的插件组。

## 查看 Workspace 信息

使用命令 `mbox status` 将会打印当前 Workspace 的状态信息。

```shell
$ mbox status
[Root]: /Users/mbox/demo

[Feature]: FreeMode

    It is empty!

[Containers]:
    It is empty!
```

## 管理 Repo

有了 Workspace 之后，就可以往 Workspace 里面添加仓库了。

更多 Repo 功能查看 [MBox 与 Repository](./Repository-cn.md)

### Git

Workspace 内每个仓库都可能需要进行 Git 操作，Git 作为开发中使用频率最高的功能，必然也是 MBox 考虑到重点，而 Workspace 内仓库数量多的时候，我们就需要批处理能力来帮助批量执行 Git 命令。

更多 Git 功能查看 [MBox 与 Git](./Git-cn.md)

## Feature

MBox 借鉴了 `GitFlow` 的 `feature/` 分支模型，描述一个需求功能开发周期内的仓库和环境。MBox 没有借鉴 `GitFlow` 的其他分支模型，例如 `bugfix/` 等，考虑到常用性和扩展性，只保留了 `feature/` 分支模型。

Feature 作为需求开发周期的核心功能，承担了不同需求的 Workspace 环境的隔离，每个 Feature 包含 仓库列表、仓库分支、本地修改、开发环境等其他信息，每个 Feature 都是独立的。通过切换 Feature 可以实现完全的沙盒隔离。

更多 Feature 功能查看 [MBox 与 Feature](./Feature-cn.md)

## Dependency Manager

当 Workspace 里有多个仓库的时候，每个仓库可能扮演不同角色。例如，有一个仓库是主工程，其他仓库是 SDK。

MBox 将主工程称之为 Container，SDK 工程称之为 Component。

在日常开发中，遇到最多的就是依赖的版本控制，包括将依赖下载到本地进行开发。MBox 在依赖管理方面做了很多工作，抽象了依赖管理工具 `MBoxDependencyManager`，进一步抽象出了 容器 和 组件 两个概念，从而可以达到更好的体验效果。

更多 Dependency 功能查看 [MBox 与 Dependency](./Dependency-Manager-cn.md)
