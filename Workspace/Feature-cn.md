# MBox 与 Feature

> `Feature` 是 Workspace 内的最小工作单元，用户可在 Workspace 内创建多个 Feature 并自由切换，有点类似 Git 的分支。

> `Feature` 是借鉴了 `GitFlow` 的 `feature/` 分支模型，描述一个需求功能开发周期内的仓库和环境。MBox 没有借鉴 `GitFlow` 的其他分支模型，例如 `bugfix/` 等，考虑到常用性和扩展性，只保留了 `feature/` 分支模型。

> 每个 Feature 包含 仓库列表、仓库分支、本地修改、开发环境等其他信息，每个 Feature 都是独立的。

有两种类型的 Feature：

1. **Free Mode** 是新建的 Workspace 的默认状态，该状态下，用户可以任意切换仓库的 HEAD 游标（切换分支等），使用 `mbox feature free` 可进入该模式。

1. **Feature Mode** 该类型 Feature 下，所有仓库的 HEAD 游标都处于同名的 Feature 分支上，而且每个仓库需要有 `TARGET BRANCH` 信息，使用 `mbox feature start [FEATURE NAME]` 可进入该模式。

不难看出，`Free Mode` 也是一种特殊的 Feature。
`Free Mode` 只有一个，即为初始的默认状态，`Feature Mode` 可以有多个，所有新创建的 Feature 都是 `Feature Mode`。

前面提到了 Feature 是描述需求开发周期，那么就一定会涉及到一个问题：从哪个分支上开始，合入哪个分支！（一般来说，从哪个分支开始，也是合入同一个分支）`TARGET BRANCH` 就是用来描述该 Feature 结束之后将合入哪个分支。

在 `Free Mode` 下，仓库的分支没有约束，也不需要 `TARGET BRANCH`；而在 `Feature Mode` 下，不仅仅严格约束了当前分支要和 Feature 名称匹配，同时还需要设置好 `TARGET BRANCH`。

### 查看 Feature 列表

我们可以使用 `mbox feature list` 来列出所有的 feature 和各自的仓库列表。

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
在这里可以看到有 2 个 Feature（分别名为 `bugfix` 和 `test`）和 1 个 FreeMode。

### 创建/切换 Feature

我们可以基于当前 Feature 来创建一个新 Feature。MBox 默认会将当前 Feature 的信息（例如仓库列表）拷贝到新的 Feature 中。

但是，有一点点差别：
1. 如果当前是 `Free Mode`，所有仓库当前分支将会作为新 Feature 的基线被切出，同时也做为 `TARGET BRANCH`。因此所有仓库必须是在一个分支上，不能是一个 Tag 或者一个 Commit（因为要作为 `TARGET BRANCH`，就必须是一个分支）。所有未提交的改动将带入到新 Feature 中。(__类似从当前 Feature 切出新 Feature__)

1. 如果当前是 Feature，所有仓库的目标分支将作为新 Feature 的基线被切出，同时也作为 `TARGET BRANCH`。同时，所有未提交的改动也将会被保存起来，不会带入到新 Feature 中。（__类似复制当前 Feature__）

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

#### mbox merge

`mbox merge` 仅能在 feature mode 中使用。

MBox 创建 Feature 的时候，会设置 `Target Branch`，这个 `目标分支` 一般用来代表该 Feature 未来即将合入的分支，一般来说，创建 Feature 的时候，都是从哪个分支创建，就会合入哪个分支，例如从 develop 分支创建的 Feature 分支，后续也会合入 develop 分支。

在日常开发过程中，经常会遇到一种场景，需要合并最新的代码，例如合并下 develop 最新代码。而这里的 `最新代码` 一般指的就是自己的 `目标分支` 的最新节点。但是由于 MBox Workspace 下存在多个仓库，每个仓库的 `Target Branch` 就会存在不同，单纯使用 `mbox git merge origin/develop` 这种类型的批处理命令无法满足（因为不是都是 develop 分支）。

MBox 单独抽象了一个命令 `mbox merge` 帮助用户快速地将各自仓库的 `目标分支的远端最新代码` merge 到当前的 feature 分支中来。

```shell
$ mbox status
[Root]: /Users/mbox/demo

[Feature]: test

    MBoxReposDemo  https://github.com/MBoxPlus/MBoxReposDemo.git  [feature/test]   ->  [main]     ↳0  ↰0
      + [CocoaPods]  MBoxReposDemo
    SnapKit        https://github.com/SnapKit/SnapKit.git         [feature/test]   ->  [develop]  ↳0  ↰0
      + [CocoaPods]  SnapKit

$ mbox merge
[MBoxReposDemo]
  Merge branch `origin/main` into branch `feature/test`.
  There is nothing to merge.
[SnapKit]
  Merge branch `origin/develop` into branch `feature/test`.
  There is nothing to merge.
```

上述例子的 Target Branch 分别是 `main` 和 `develop`，`mbox merge` 命令会自动使用各自仓库的 Target Branch 合并。
