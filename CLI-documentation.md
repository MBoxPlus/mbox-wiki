# Global

### mbox init

```shell
$ mbox init [PLUGIN_GROUP [...]]
```

Init Workspace

### Options

<table>
  <tr>
    <td>--plugin</td>
    <td>Config the plugin. It could be used many times to config more plugins.</td>
  </tr>
  <tr>
    <td>--name</td>
    <td>To set the new MBox workspace (folder) name.</td>
  </tr>
</table>

### mbox setup

```shell
$ mbox setup
```

Setup Command Line Tool

### Options

<table>
  <tr>
    <td>--bin-dir</td>
    <td>Output the executable bin to specific directory. Default value: `/usr/local/bin`</td>
  </tr>
</table>

# Workspace

### mbox activate

```shell
$ mbox activate NAME
```

Activate a or more components.

#### Arguments

<table>
  <tr>
    <td>NAME</td>
    <td>Repo/Component Name</td>
  </tr>
</table>


#### Example

```shell
$ mbox activate repo1
$ mbox activate repo1/component1
```

### mbox add

```shell
$ mbox add NAME [TARGET_BRANCH] [BASE_BRANCH]
```

Add a repo into current feature

#### Arguments

<table>
  <tr>
    <td>NAME</td>
    <td>Repo Name/URL/Path</td>
  </tr>
   <tr>
    <td>TARGET_BRANCH</td>
    <td>[Optional] Merge to the target branch</td>
  </tr>
   <tr>
    <td>BASE_BRANCH</td>
    <td>[Optional] Check from the base branch. Defaults same as TARGET_BRANCH</td>
  </tr>
</table>

#### Options

<table>
  <tr>
    <td>--mode</td>
    <td>Use `copy`/`move`/`worktree` to handle local path </td>
  </tr>
  <tr>
    <td>--component</td>
    <td>Activate a component, only for `URL`/`PATH`</td>
  </tr>
  <tr>
    <td>--product-json</td>
    <td>Repo-related product info</td>
  </tr>
  <tr>
    <td>--checkout-from-commit</td>
    <td>Checkout the feature branch from the commit instead of the latest base branch. It only works in a feature.</td>
  </tr>
  <tr>
    <td>--recurse-submodules</td>
		<td>After the clone is created, initialize all submodules within, using their default settings.</td>
  </tr>
  <tr>
    <td>--keep-local-changes</td>
    <td>Keep local changes when add a local repository with copy/move mode.</td>
  </tr>
</table>

#### Example

```shell
$ mbox add ./../repo1  # add ../../repo1 into current feature
```

### mbox bundle

```shell
$ mbox bundle COMMAND [--no-color] [--verbose] [ARGS]
```

Redirect to Bundler with MBox environment


#### Example

```shell
$ mbox bundle update #  Update your gems to the latest available versions
```

### mbox config

```shell
$ mbox config [NAME] [VALUE]
```

 Get/Set Default Configuration

#### Arguments

<table>
  <tr>
    <td>NAME</td>
    <td>[Optional] Config Name</td>
  </tr>
    <tr>
    <td>VALUE</td>
    <td>[Optional] Config Value</td>
  </tr>
</table>


#### Example

```shell
$ mbox config  #  Get Default Configuration

{
  "plugins" : {
    "MBoxCocoapods" : {

    },
    "MBoxCocoapodsIntegrate" : {

    },
    "MBoxDevices" : {

    },
    "MBoxMPaaS" : {

    }
  },
  "plugins2" : {

  }
}
```

### mbox container

```shell
$ mbox container
```

Manage Container

#### Commands

<table>
  <tr>
    <td>+ disuse</td>
    <td>Switch container in current feature</td>
  </tr>
  <tr>
    <td>+ list</td>
    <td>List available containers in current feature</td>
  </tr>
  <tr>
    <td>+ use</td>
    <td>Switch container in current feature</td>
  </tr>
</table>


#### Example

```shell
$ mbox container list  # List available containers in current feature

List available containers:
      MBoxReposDemo  Bundler  
```

### mbox deactivate

```shell
$ mbox deactivate NAME
```

Deactivate a or more components

#### Arguments

<table>
  <tr>
    <td>NAME</td>
    <td>Repo/Component Name</td>
  </tr>
</table>


#### Example

```shell
$ mbox deactivate repo1 
$ mbox deactivate repo1/component1
```

### mbox depend

```shell
$ mbox depend [NAME]
```

Show/Change dependencies

#### Arguments

<table>
  <tr>
    <td>NAME</td>
    <td>[Optional] The dependency name</td>
  </tr>
</table>


#### Example

```shell
$ mbox depend  # Show dependencies
```

### mbox env

```shell
$ mbox env
```

Show mbox environment


#### Example

```shell
$ mbox env  # Show Mbox environment
```

### mbox exec 

```shell
$ mbox exec [OPTIONS] [ARGUMENTS...]
```

exec command line in MBox environment

#### Example

```shell
$ mbox exec echo HelloWorld

HelloWorld
```

### mbox feature

```shell
$ mbox feature
```

Manage Features

#### Commands

<table>
  <tr>
    <td>+ clean</td>
    <td>Clean merged feature</td>
  </tr>
  <tr>
    <td>+ export</td>
    <td>Export a json from feature</td>
  </tr>
  <tr>
    <td>+ merge</td>
    <td>Create a MR to target branch</td>
  </tr>
  <tr>
    <td>+ finish</td>
    <td>Finish current feature</td>
  </tr>
  <tr>
    <td>+ free</td>
    <td>Switch to the `Free Mode` feature</td>
  </tr>
  <tr>
    <td>+ import</td>
    <td>Import a json/url as a feature</td>
  </tr>
  <tr>
    <td>+ list</td>
    <td>List all features</td>
  </tr>
  <tr>
    <td>+ remove</td>
    <td>Remove a feature</td>
  </tr>
  <tr>
    <td>+ start</td>
    <td>Create a new feature, or continue a exist feature</td>
  </tr>
</table>


#### Example

```shell
$ mbox feature free  # Switch to the `Free Mode` feature
```

### mbox gem

Redirect to Gem with MBox environment

```shell
$ mbox gem COMMAND [ARGUMENTS...] [OPTIONS...]
```

### mbox git

Execute git command for every repo

```shell
$ mbox git COMMAND
```

Execute git command for every repo

#### Commands

<table>
  <tr>
    <td>+ config</td>
    <td>Execute git command for every repo</td>
  </tr>
  <tr>
    <td>+ hooks</td>
    <td>Show/Set the git hooks for workspace</td>
  </tr>
</table>


#### Arguments

<table>
  <tr>
    <td>COMMAND</td>
    <td>The command will be executed</td>
  </tr>
</table>


#### Example

```shell
$ mbox git status  # Show the working tree status fot every repo

[MBoxReposDemo]
  On branch main
  Your branch is up to date with 'origin/main'.

  Changes not staged for commit:
    (use "git add/rm <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
  	deleted:    Example/Pods/Alamofire/LICENSE
  	deleted:    Example/Pods/Alamofire/README.md
  	 Untracked files:
    (use "git add <file>..." to include in what will be committed)
  	Example/Pods

  	no changes added to commit (use "git add" and/or "git commit -a")
[SnapKit]
  	HEAD detached at 5.0.1
  	nothing to commit, working tree clean
```

---

### mbox git-sheet

Execute git command for every repo and format output

```shell
$ mbox git-sheet
```

Show git status for every repo

#### Commands

<table>
  <tr>
    <td>+ fetch</td>
    <td>Perform git fetch for every repo</td>
  </tr>
  <tr>
    <td>+ pull</td>
    <td>Perform git pull for every repo</td>
  </tr>
  <tr>
    <td>+ push</td>
    <td>Perform git push for every repo</td>
  </tr>
  <tr>
    <td>+ status</td>
    <td>Show git status for every repo</td>
  </tr>
</table>


#### Example

```shell
$ mbox git-sheet fetch  # Perform git fetch for every repo

Fetching MBoxReposDemo
Fetching SnapKit

    MBoxReposDemo  [feature/featureTest]*  ->  main     ↳1  ↰0
    SnapKit        [feature/featureTest]   ->  develop  ↳0  ↰0
```

---

### mbox go

```shell
$ mbox go [NAME]
```

Quickly open workspace

#### Arguments

<table>
  <tr>
    <td>NAME</td>
    <td>[Optional] Specific workspace file to open</td>
  </tr>
</table>


#### Example

```shell
$ mbox go # Quick open mboxDemoTest.xcworkspace

Open `/workspace/mboxDemoTest.xcworkspace`
```

---

### mbox merge

```shell
$ mbox merge [NAME]
```

Merge \`other feature\`/\`target branch` into current feature

#### Arguments

<table>
  <tr>
    <td>NAME</td>
    <td>[Optional] Merge the Feature into current feature</td>
  </tr>
</table>


#### Example

``` shell
$ mbox merge featureTest  # Merge branch `other branch` into branch featureTest

[MBoxReposDemo]
  Merge branch `origin/main` into branch `feature/featureTest`.
  Merge Done!
[SnapKit]
  Merge branch `origin/develop` into branch `feature/featureTest`.
  There is nothing to merge.
```

---

### mbox new

```shell
$ mbox new NAME [BRANCH]
```

Create a project in workspace

#### Arguments

<table border="1">
  <tr>
    <td>NAME</td>
    <td>Project Name</td>
  </tr> 
  <tr>
    <td>BRANCH</td>
    <td>[Optional] Create the branch</td>
  </tr>
</table>


#### Example

```shell
$ mbox new test  # Create a test project in workspace

Init git repository
Checkout workspace
Show Status
  [Root]: /workspace

  [Feature]: featureTest

      MBoxReposDemo  https://github.com/MBoxPlus/MBoxReposDemo  [feature/featureTest]*  ->  [main]     ↳0  ↰0
        + [CocoaPods]  MBoxReposDemo
      SnapKit        https://github.com/SnapKit/SnapKit.git     [feature/featureTest]   ->  [develop]  ↳0  ↰0
        + [CocoaPods]  SnapKit
      test                                                      [feature/featureTest]   ->  [master]

  [Containers]:
   => MBoxReposDemo  Bundler + CocoaPods
```

---

### mbox open

```shell
$ mbox open [PATHS [...]]
```

Open specific path in MBox Environment

#### Arguments

<table>
  <tr>
    <td>PATHS</td>
    <td>[Optional] Specific Path</td>
  </tr>
</table>


#### Example

```shell
$ mbox open  # Open workspace path in MBox Environment

Open `/workspace`
```

---

### mbox plugin

```shell
$ mbox plugin
```

Manage Plugins

#### Commands

<table>
  <tr>
    <td>+ disable</td>
    <td>Disable plugins by name</td>
  </tr>
  <tr>
    <td>+ enable</td>
    <td>Enable plugins by name</td>
  </tr>
  <tr>
    <td>+ launch</td>
    <td>Run a plugin launcher</td>
  </tr>
  <tr>
    <td>+ list</td>
    <td>List all plugins</td>
  </tr>
  <tr>
    <td>+ uninstall</td>
    <td>Uninstall Plugins</td>
  </tr>
</table>


#### Example

```shell
$ mbox plugin disable cocopods  # Disable cocoapods plugin

Modify file: `/workspace/.mboxconfig`
  Disable plugin `cocopods` success!
```

---

### mbox pod

```shell
$ mbox pod
```

Redirect to Bundler with MBox environment

```shell
$ pod COMMAND
```

CocoaPods, the Cocoa library package manager

#### Commands

<table>
  <tr>
    <td>+ cache</td>
    <td>Manipulate the CocoaPods cache</td>
  </tr>
  <tr>
    <td>+ deintegrate</td>
    <td>Deintegrate CocoaPods from your project</td>
  </tr>
  <tr>
    <td>+ env</td>
    <td>Display pod environment</td>
  </tr>
  <tr>
    <td>+ init</td>
    <td>Generate a Podfile for the current directory</td>
  </tr>
  <tr>
    <td>+ install</td>
    <td>Install project dependencies according to versions from a Podfile.lock</td>
  </tr>
  <tr>
    <td>+ ipc</td>
    <td>Inter-process communication</td>
  </tr>
  <tr>
    <td>+ lib</td>
    <td>Develop pods</td>
  </tr>
  <tr>
    <td>+ list</td>
    <td>List pods</td>
  </tr>
  <tr>
    <td>+ mbox</td>
    <td>MBox support</td>
  </tr>
  <tr>
    <td>+ outdated</td>
    <td>Show outdated project dependencies</td>
  </tr>
  <tr>
    <td>+ plugins</td>
    <td>Show available CocoaPods plugins</td>
  </tr>
  <tr>
    <td>+ repo</td>
    <td>Manage spec-repositories</td>
  </tr>
  <tr>
    <td>+ search</td>
    <td>Search for pods</td>
  </tr>
  <tr>
    <td>+ setup</td>
    <td>Setup the CocoaPods environment</td>
  </tr>
  <tr>
    <td>+ spec</td>
    <td>Manage pod specs</td>
  </tr>
  <tr>
    <td>+ trunk</td>
    <td>Interact with the CocoaPods API (e.g. publishing new specs)</td>
  </tr>
  <tr>
    <td>+ try</td>
    <td>Try a Pod!</td>
  </tr>
  <tr>
    <td>+ update</td>
    <td>Update outdated project dependencies and create new Podfile.lock</td>
  </tr>


#### Options

<table>
  <tr>
    <td>--allow-root</td>
    <td>Allows CocoaPods to run as root</td>
  </tr>
  <tr>
    <td>--silent</td>
    <td>Show nothing</td>
  </tr>
  <tr>
    <td>--version</td>
    <td>Show the version of the tool</td>
  </tr>
  <tr>
    <td>--verbose</td>
    <td>Show more debugging information</td>
  </tr>
  <tr>
    <td>--no-ansi</td>
		<td>Show output without ANSI codes</td>
  </tr>
  <tr>
    <td>--help</td>
    <td>Show help banner of specified command</td>
  </tr>
</table>


#### Example

```shell
$ mbox pod install  # Install project dependencies according to versions from a Podfile.lock

[MBox] Redirect component `MBoxReposDemo` -> `MBoxReposDemo`
[MBox] Redirect component `SnapKit` -> `SnapKit`
Re-creating CocoaPods due to major version update.
Analyzing dependencies
Downloading dependencies
Installing SnapKit 5.0.1
Generating Pods project
Integrating client project
Pod installation complete! There are 3 dependencies from the Podfile and 3 total pods installed.
```

---

### mbox remove

```shell
$ mbox remove NAME [...]
```

Remove project from workspace

#### Arguments

<table>
  <tr>
    <td>NAME</td>
    <td>Repo Name</td>
  </tr>
</table>


#### Example

```shell
$ mbox remove test  # Remove test project from workspace

Validate Repo
Remove Repo
Show Status
  [Root]: /workspace/mboxDemoTest

  [Feature]: featureTest

      MBoxReposDemo  https://github.com/MBoxPlus/MBoxReposDemo  [feature/featureTest]*  ->  [main]     ↳0  ↰0
        + [CocoaPods]  MBoxReposDemo
      SnapKit        https://github.com/SnapKit/SnapKit.git     [feature/featureTest]   ->  [develop]  ↳0  ↰0
        + [CocoaPods]  SnapKit

  [Containers]:
   => MBoxReposDemo  Bundler + CocoaPods
```

---

### mbox repo

```shell
$ mbox repo
```

Manage Repos

#### Commands

<table>
  <tr>
    <td>+ search</td>
    <td>Manage Repos</td>
  </tr>
</table>


#### Example

```shell
$ mbox repo search MBoxReposDemo  # Search the information of repo

base_branch: main
base_type: branch
components: []
full_name: MBoxReposDemo@MBoxPlus
name: MBoxReposDemo
owner: MBoxPlus
target_branch: main
url: https://github.com/MBoxPlus/MBoxReposDemo
```

---

### mbox setup

```shell
$ mbox setup
```

Setup Command Line Tool

#### Example

```shell
$ mbox setup  # Setup Command Line Tool

Install mbox in `/usr/local/bin`
Source mbox function in `~/.profile`

Setup Completed.
```

---

### mbox status

```shell
$ mbox status [NAME]
```

Show Status

#### Arguments

<table>
  <tr>
    <td>NAME</td>
    <td>[Optional] Show Other Feature</td>
  </tr>
</table>


#### Example

```shell
$ mbox status  # Show Status

[Root]: /workspace

[Feature]: featureTest

    MBoxReposDemo  https://github.com/MBoxPlus/MBoxReposDemo  [feature/featureTest]*  ->  [main]     ↳0  ↰0
      + [CocoaPods]  MBoxReposDemo
    SnapKit        https://github.com/SnapKit/SnapKit.git     [feature/featureTest]   ->  [develop]  ↳0  ↰0
      + [CocoaPods]  SnapKit

[Containers]:
 => MBoxReposDemo  Bundler + CocoaPods
```

---
