## Create a Workspace

Use command `mbox init [PLUGIN_GROUP]` to create a workspace. For example, `mbox init ios` will make the current directory become a MBox workspace.

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

Type command `mbox status` will display the current status of the workspace.

```shell
$ demo mbox status
[Root]: /Users/mbox/demo

[Feature]: FreeMode

    It is empty!

[Containers]:
    It is empty!
```

`mbox --help` or `mbox [SUB_COMMAND] --help` will output brief documentation.

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

## Manage Repos

### Add Repo

Use command `mbox add` to clone a repo into current feature. 

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

In [free mode](https://github.com/MBoxPlus/mbox/wiki/MBox-terminology#free-mode-%E5%92%8C-feature-mode), there are **2** arguments for the command `mbox add`，the 1st is the git URL and the 2nd is the branch/tag name which is used to checkout.

In [feature mode](https://github.com/MBoxPlus/mbox/wiki/MBox-terminology#free-mode-%E5%92%8C-feature-mode), there are **3** arguments. The 1st is the git URL. The 2nd is the base branch/tag/commit, which is what the feature branch is checked out from. The 3rd is the target branch which is used for merge request.

### Remove Repo

Use command `mbox remove` to remove a repo from the current feature.

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

Remove the repo called `SnapKit` from the feature `test`. All the operations on the repo just have an effect on the current feature.

### Git

#### mbox git

`mbox git` helps manage many git repositories by executing git command for each repository. `mbox git` is based on [libgit2](https://github.com/libgit2/libgit2) and not meant to replace git. You can still use git to manage the repository under the MBox workspace.

```shell
# Execute `pull` in each repo
$ mbox git pull
[mbox-core]
  Already up to date.
[mbox-git]
  Already up to date.

# Execute `fetch` command in each repo
$ mbox git fetch

# Go to each repository to checkout to master branch
$ mbox git checkout master
[mbox-core]
  Updating files: 100% (6214/6214), done.
  Switched to branch 'master'
  Your branch is up to date with 'origin/master'.
[mbox-git]
  Switched to branch 'master'
  Your branch is up to date with 'origin/master'.

# Discard the changes of all repositories
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

It is similar to `mbox git`. The output of the git command are nicely formatted with a tabular form.

Supported command：

- fetch
- pull
- push
- status

Example:

```shell
$ mbox git-sheet status
    mbox-core                [master]   ↑0  ↓0
    mbox-ruby                [master]   ↑0  ↓0
    mbox-container           [master]*  ↑0  ↓0
```

`mbox merge` is another git related command, which only can be used in feature mode. This command helps you to make it easier to the merge target branch into the current feature branch.

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

## Manage Features

### List Features

We can use `mbox feature list` to list all the features of the current workspace.

```shell
$ mbox feature list
[bugfix]
  MBoxReposDemo
[FreeMode]
  MBoxReposDemo
  SnapKit
[test]
  MBoxReposDemo
```
There are 2 features (named 'bugfix' and 'test') and a free-mode in this workspace.

### Create Feature

We can create a new feature base on free-mode or another feature and copy a list of repos to the new workspace. However, there are some differences between the two cases. 

If the new workspace is based on free-mode, the base commit to checkout is the git HEAD of the free-mode's repos. If based on another feature, the base commit of the new feature to checkout is the target branch reference of the previous feature. Also, all of the git changes will be stashed if create one feature from the other feature.

```shell
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

### Remove Feature

Use command `mbox feature remove` to remove a feature.

## Multiple Containers

Multiple containers are allowed in a MBox feature. We can use `mbox container use/disuse` to switch between containers.

```shell
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

This workspace have 2 containers `MBoxReposDemo` and `MBoxReposDemo2` and we switched the current container from `MBoxReposDemo` to `MBoxReposDemo2`. MBox will now reintegrate the `MBoxReposDemo2` project when do `mbox pod install`.

We have to choose one container before run project integration command, such as `mbox pod install` or `mbox bundle install`.