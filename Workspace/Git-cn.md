# MBox 与 Git 

MBox 使用标准 git 命令下载仓库，同时使用 libgit2 进行 git 数据查询操作，因此 MBox 下载的所有仓库都能直接用任意的 Git 客户端进行操作，无需担心兼容性问题。

## Worktree

MBox 使用了 Git Worktree 技术，将 Git 仓库和工作副本分离开，防止用户意外删除仓库导致未提交的代码丢失。目前市面上所有 Git 客户端均能很好的支持 Worktree 技术。

Worktree 相较于普通项目来说，最显著的变化就是，用户看到的仓库（实际上是工作副本）的根目录下的 `.git` 不再是目录，而是一个文件，文件内容描述了仓库的具体路径。

由于 `.git` 目录变为了 `.git` 文件，部分兼容性不够的脚本在获取仓库路径的时候，写死了路径，导致读写文件失败，需要提升对 Worktree 模式的兼容性。

Worktree 不支持移动项目，因为所有的配置里面都是绝对路径。如果用户更改了 Workspace 名称或者移动了 Workspace 路径，MBox 提供了一个命令来自动修复：`mbox git repair`.

## mbox git

在 Workspace 下有多个仓库时，往往需要同时对多个仓库进行相同的 Git 操作。MBox 提供了 Git 批处理命令，对所有仓库进行批量的执行同一个 Git 命令。

```shell
$ mbox git COMMAND
```

循环在每个仓库下执行 `git COMMAND` 命令：

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

## mbox git-sheet

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

## Git Hooks

Git Hooks 指的是 Git 在特定事件发生之前或之后执行特定脚本代码，Git 规范是在项目的根目录下的 `.git/hooks` 目录下存放对应的脚本文件，例如：

```shell
❯ ls .git/hooks
applypatch-msg     post-update        pre-push           prepare-commit-msg
commit-msg         pre-applypatch     pre-rebase         update
fsmonitor-watchman pre-commit         pre-receive
```

**注意：这些脚本必须具有可执行权限！**

MBox 提供了 Workspace 级别的 Git Hooks 统一管理能力，同一个 Hook 会按照以下顺序轮流执行：

1. [可共享][随插件分发] 运行插件内置 Git Hooks，影响所有仓库 ( `Plugin/Resources/gitHooks` )
2. [不共享][用户本地] 运行 Workspace 目录下配置 Hooks，影响所有仓库 ( `Workspace/.mbox/git/hooks` )
3. [可共享][仓库共享] 运行仓库内配置 Hooks 目录，可提交到 Git，影响单个仓库 ( `repo/gitHooks` )
4. [不共享][仓库本地] 运行仓库原生 Hooks 目录，不可提交到 Git，影响单个仓库 ( `repo/.git/hooks` )

如果某个脚本执行失败（返回状态码非0），则终止执行。

**注意：上述功能在 MBox 之外，使用其他 Git 客户端或者命令行，对 Workspace 下所有仓库始终有效**

**注意：Hook 脚本请务必保证有可执行权限！**

### LFS 兼容性

MBox 通过自带 Git Hook 分发脚本，将所有 Hooks 按顺序执行。然而部分第三方工具也会添加 Git Hooks 脚本，例如 `git-lfs`。在第三方工具读取 Git Hooks 路径的时候，会由于 MBox 的重定向，读取到 MBox 的路径，最终导致无法安装第三方脚本。

MBox 提供了关闭 Git Hooks 的命令：`mbox git hooks --disable`。

安装第三方工具的 Hooks 前，务必先关闭 MBox 的 Git Hooks，防止 MBox 的文件被破坏。安装完 Hooks 之后，可重新开启 MBox 的 Git Hooks：

```bash
# 执行提示 Hook 未安装
$ git commit -m "add demo"
  This should be run through Git's post-merge hook.  Run `git lfs update` to install it.

# 先关闭 MBox 的 Git Hooks
$ mbox git hooks --disable

# 安装 LFS
$ git lfs install

# 重新开启 MBox 的 Git Hooks
$ mbox git hooks --enable
```

## Git Config

MBox 既然管理了多个仓库，能够批量的操作每个仓库，那自然还希望能统一控制所有仓库的配置。因此 MBox 在 Git 的 `仓库级别配置`、`用户全局配置`、`系统全局配置` 中间，新增了 `Workspace 级别配置`。

MBox Hook 了 `git config` 命令，增加了 `--workspace` 参数，这样就可以读写 Workspace 级别的配置了。

有什么用途呢？最简单的用法就是，可以配置用户信息，让每个 Workspace 的所有仓库使用统一的提交邮箱（例如公司账户），方便的区分个人和企业项目：

```shell
$ mbox git config --workspace user.name "Your Name"
$ mbox git config --workspace user.email "youremail@yourdomain.com"
$ mbox git config --workspace
  push.default=current
  core.hookspath=/Applications/MBox.app/Contents/Resources/Plugins/MBoxWorkspace/Resources/gitHookManager
  user.name=Your Name
  user.email=youremail@yourdomain.com
```

## noblob clone

随着项目的不断迭代，仓库的历史数据也在不断增长，导致每次 clone 仓库可能需要下载很大的历史数据。

MBox 综合考量了 日志、数据、工作副本 之间的关系，Git Clone 时引入了 noblob 模式，下载日志和工作副本，去除历史数据，减小下载体积，从而节省磁盘空间和提升下载速度。

noblob 模式需要 Git 服务端支持（要求 Git 版本 >= 2.27.0），公共服务器一般都支持（例如 Github）。

可以使用以下命令判断服务端的 Git 版本：

```
$ GIT_TRACE_PACKET=true git ls-remote --heads 任意一个GIT仓库地址 |& grep \<\ agent
12:33:28.768783 pkt-line.c:80           packet:    ls-remote< agent=git/2.27.0
```

输出的 `agent=git/2.27.0` 代表服务端使用的 Git 版本为 `2.27.0`.

noblob 功能默认是开启的，且会尝试检测服务器的 Git 版本，不满足版本要求则会自动禁用。

如果需要完全关闭该功能，可以使用命令设置：`mbox config git.noblob_clone false`

```shell
# noblob clone 关闭时
$ mbox config git.noblob_clone false
$ mbox add git@github.com:libgit2/libgit2.git main
$ du -sh .mbox/repos/libgit2@libgit2
 63M	.mbox/repos/libgit2@libgit2

# 开启 noblob clone
$ mbox remove libgit2 --include-repo
$ mbox config git.noblob_clone true

# noblob clone 开启后
$ mbox add git@github.com:libgit2/libgit2.git main
$ du -sh .mbox/repos/libgit2@libgit2
 20M	.mbox/repos/libgit2@libgit2
```

启用了 noblob 模式后，仓库体积可以极大的减小仓库下载体积，优化用户首次下载流程的体验，因此以下命令会得到提升：

- git pull/fetch

以下命令不会受到影响：

- 和历史数据无关的操作，包括 git status/git commit/git push 等
- git log 和 git log -- path 以及 GUI 的历史树渲染

以下命令性能受到轻微影响：

- git show <commit> 以及 GUI 的显示某个 Commit 的具体修改内容
- git checkout 以及 GUI 的切换分支
- git merge 以及 GUI 的 Merge 功能

以下命令性能有较大影响：

- git log --stat
- git blame 以及 GUI 的显示指定文件的每一行的提交信息
- git diff <commit>...<commit>

Xcode 等显示文件 Blame/Author 功能会受到很大影响，为了平衡性能和用户需求，MBox 额外提供后台还原完整仓库的功能：在执行完仓库 Clone 之后，会后台 Fork 一个新的 git 进程，进行下载完整仓库，并且会有系统通知提示用户下载完成。

MBox 提供配置开关该功能：

```
# 关闭 Partial Clone 功能，直接下载完整仓库
mbox config git.partial_clone false

# 关闭后台下载完整仓库功能，如果未关闭 partial_clone，则仓库保留在 partial_clone 状态
mbox config git.full_clone false
```
