# MBox 与 Repository

有了 Workspace 之后，就可以往 Workspace 里面添加仓库了。

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

`mbox add` 命令接收的参数比较多样，支持多种方式：

1. `mbox add GIT_URL [BRANCH]` 添加远程仓库并切换到 `BRANCH` 分支
1. `mbox add PATH [BRANCH] [-mode=copy|move|worktree]` 添加本地仓库，并切换到 `BRANCH` 分支，使用 `copy`/`move`/`worktree` 模式
1. `mbox add NAME` 添加一个组件，会根据 NAME 分析上下文和依赖树搜索组件

### 删除 Repo

可使用 `mbox remove` 删除仓库。

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
 
 # 从上面 status 中可以看到 SnapKit，下面我们使用 remove 命令将其移除
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
