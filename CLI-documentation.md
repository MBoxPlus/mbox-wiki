## mbox activate

```
$ mbox activate NAME
```
Activate a or more components

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      Repo/Component Name
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --tool
    </td>
    <td>
      Use the specified dependency management tool
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --all
    </td>
    <td>
      All Components
    </td>
  </tr>
</table>

#### Example:
```bash
# Activate all components in the `repo1`
$ mbox activate repo1

# Activate the component named `component1` in the `repo1`
$ mbox activate repo1/component1
```


## mbox add

```
$ mbox add NAME [TARGET_BRANCH] [BASE_BRANCH]
```
Add a repo into current feature

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      Repo Name/URL/Path
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      TARGET_BRANCH
    </td>
    <td>
      [Optional] Merge to the target branch
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      BASE_BRANCH
    </td>
    <td>
      [Optional] Check from the base branch. Defaults same as TARGET_BRANCH
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --component
    </td>
    <td>
      Activate a component, only for `URL`/`PATH`
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --mode
    </td>
    <td>
      Use `copy`/`move`/`worktree` to handle local path
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --product-json
    </td>
    <td>
      Repo-related product info
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --tool
    </td>
    <td>
      Use the specified dependency management tool
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --activate-all-components
    </td>
    <td>
      Activate all components. Default value will be true if add a repo, while default value will be false if add a component. Use `mbox config dependency_manager.activate_all_components_after_add_repo true/false` to change the default behavior.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --checkout-from-commit
    </td>
    <td>
      Checkout the feature branch from the commit instead of the latest base branch. It only works in a feature.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --keep-local-changes
    </td>
    <td>
      Keep local changes when add a local repository with copy/move mode.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --recurse-submodules
    </td>
    <td>
      After the clone is created, initialize all submodules within, using their default settings.
    </td>
  </tr>
</table>

#### Example:
```bash
# Copy a relative path into current feature, and keep the local changes.
$ mbox add ../repo1 --mode copy --keep-local-changes

# Download a remote url into current feature
$ mbox add git@github.com:xx/xxx.git

# Download a repository with a name, the name maybe a component name or a repository name
$ mbox add AFNetworking
```


## mbox bundle

```
$ mbox bundle
```
Redirect to Bundler with MBox environment

#### Example:
```bash
# Install gems with bundler
$ mbox bundle install
```


## mbox config

```
$ mbox config [NAME] [VALUE]
```
Get/Set Default Configuration

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Config Name
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      VALUE
    </td>
    <td>
      [Optional] Config Value
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      -d
    </td>
    <td style="white-space: nowrap">
      --delete
    </td>
    <td>
      Delete configuration to restore default value.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      -g
    </td>
    <td style="white-space: nowrap">
      --global
    </td>
    <td>
      Apply to global configuration.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
    </td>
    <td style="white-space: nowrap">
      --rc
    </td>
    <td>
      Apply to `~/.mboxrc`.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      -w
    </td>
    <td style="white-space: nowrap">
      --workspace
    </td>
    <td>
      Use workspace setting
    </td>
  </tr>
</table>

#### Example:
```bash
# Get a value from gloal configuration
$ mbox config key --global

# Show all configs
$ mbox config

# Set a configuration in global configuration
$ mbox config key value --global

# Delete a configuation in global configuration
$ mbox config key --delete
```


## mbox container disuse

```
$ mbox container disuse NAME
```
Switch container in current feature

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      Container Name
    </td>
  </tr>
</table>

## mbox container list

```
$ mbox container list
```
List available containers in current feature

#### Example:
```bash
# List all containers in current feature:
$ mbox container list
List avaliable containers:
    MBoxReposDemo  Bundler
```


## mbox container use

```
$ mbox container use NAME
```
Switch container in current feature

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      Container Name
    </td>
  </tr>
</table>

## mbox deactivate

```
$ mbox deactivate NAME
```
Deactivate a or more components

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      Repo/Component Name
    </td>
  </tr>
</table>

#### Example:
```bash
# Deactivate all components in the `repo1`
$ mbox deactivate repo1

# Deactivate the component named `component1` in the `repo1`
$ mbox deactivate repo1/component1
```


## mbox depend

```
$ mbox depend [NAME]
```
Show/Change dependencies

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] The dependency name
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --branch
    </td>
    <td>
      Set git branch
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --commit
    </td>
    <td>
      Set git commit
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --git
    </td>
    <td>
      Set git url
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --path
    </td>
    <td>
      Set local path
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --source
    </td>
    <td>
      Set source
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --tag
    </td>
    <td>
      Set git tag
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --tool
    </td>
    <td>
      Use the specified dependency management tool
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --version
    </td>
    <td>
      Set version
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --binary
    </td>
    <td>
      Use binary version
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --reset
    </td>
    <td>
      Reset to default version
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --show-all
    </td>
    <td>
      Show all dependencies
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --show-changes
    </td>
    <td>
      Show changed dependencies by MBox and other tools.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --source
    </td>
    <td>
      Use source version
    </td>
  </tr>
</table>

#### Example:
```bash
# Change a dependency version
$ mbox depend AFNetworking --version 2.0

# Show all changed dependencies
$ mbox depend
AFNetworking: version 2.0
```


## mbox doc

```
$ mbox doc
```
Output all commands

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --output-path
    </td>
    <td>
      Output to the file
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --markdown
    </td>
    <td>
      Output with markdown format
    </td>
  </tr>
</table>

## mbox env

```
$ mbox env
```
Show MBox Environment

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --only
    </td>
    <td>
      Only show information. Avaliable: ROOT/SYSTEM/PLUGINS
    </td>
  </tr>
</table>

## mbox exec

```
$ mbox exec
```
Exec command line in MBox Environment

#### Example:
```bash
# echo the `pwd` in workspace
$ mbox exec pwd

# echo environment variable
$ mbox exec printenv
```


## mbox feature clean

```
$ mbox feature clean
```
Clean merged feature

## mbox feature export

```
$ mbox feature export [NAME]
```
Export a json from feature

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] The feature name will be exported.
    </td>
  </tr>
</table>

## mbox feature merge

```
$ mbox feature merge
```
Create a MR to target branch

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --force
    </td>
    <td>
      Force create the MR if there are some changes not be committed
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --forward
    </td>
    <td>
      Create the MR with repos which forward target branch
    </td>
  </tr>
</table>

## mbox feature finish

```
$ mbox feature finish
```
Finish current feature

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --force
    </td>
    <td>
      Force remove current feature if there are some changes
    </td>
  </tr>
</table>

## mbox feature free

```
$ mbox feature free
```
Switch to the `Free Mode` feature

## mbox feature import

```
$ mbox feature import STRING
```
Import a json/url as a feature

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      STRING
    </td>
    <td>
      Feature JSON/URL
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --name
    </td>
    <td>
      Use the name to override the name from the json
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --check-branch-exists
    </td>
    <td>
      Check if the feature branch exists
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --keep-changes
    </td>
    <td>
      Create a new feature with local changes
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --recurse-submodules
    </td>
    <td>
      After the clone is created, initialize all submodules within, using their default settings.
    </td>
  </tr>
</table>

## mbox feature list

```
$ mbox feature list
```
List all features

## mbox feature remove

```
$ mbox feature remove [NAME]
```
Remove a feature

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Feature Name
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --all
    </td>
    <td>
      remove all feature, will not remove current feature and FreeMode
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --force
    </td>
    <td>
      Force remove the feature if there are unmerged commits
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --include-repo
    </td>
    <td>
      remove cached repo if the repo is not used by other features
    </td>
  </tr>
</table>

## mbox feature start

```
$ mbox feature start NAME
```
Create a new feature, or continue a exist feature

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      Feature Name
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --dependencies
    </td>
    <td>
      Create a new feature with a custom dependency list. It is a JSON String, eg: {"Aweme": {"version": "1.0"}}
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --prefix
    </td>
    <td>
      Create a new feature with a custom branch prefix. Default is `feature/`, use `--prefix=` to disable it.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --repos
    </td>
    <td>
      Create a new feature with a custom repo list. It is a JSON String, eg: {"Aweme": "develop"} or {"Aweme": {"base": "0ABCD", "base_type": "commit", "target_branch": "develop"}}
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --checkout-from-remote
    </td>
    <td>
      Create a new feature branch from remote base branch, it will fetch remote. Defaults is true if current is in a feature.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --clear
    </td>
    <td>
      Create a new feature with a empty workspace
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --keep-changes
    </td>
    <td>
      Create a new feature with local changes
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --pull
    </td>
    <td>
      Pull remote branch after finish
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --recurse-submodules
    </td>
    <td>
      After the clone is created, initialize all submodules within, using their default settings.
    </td>
  </tr>
</table>

## mbox fork

```
$ mbox fork [PATHS [...]]
```
Quckly open git repository in the Fork app.

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      PATHS
    </td>
    <td>
      [Optional] Specific Path
    </td>
  </tr>
</table>

## mbox gem

```
$ mbox gem
```
Redirect to Gem with MBox environment

## mbox git

```
$ mbox git COMMAND
```
Execute git command for every repo

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      COMMAND
    </td>
    <td>
      The command will be executed
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --no-repo
    </td>
    <td>
      Exclude a repo, use this option multiple times to exclude multiple repos. Avaliable: mbox-core/mbox-git/mbox-workspace/mbox-dependency-manager/mbox-ruby/mbox-container/mbox-cocoapods/mbox-dev/mbox-dev-native/mbox-dev-ruby
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --repo
    </td>
    <td>
      Specify a repo, use this option multiple times to specify multiple repos. Avaliable: mbox-core/mbox-git/mbox-workspace/mbox-dependency-manager/mbox-ruby/mbox-container/mbox-cocoapods/mbox-dev/mbox-dev-native/mbox-dev-ruby
    </td>
  </tr>
</table>

## mbox git config

```
$ mbox git config COMMAND
```
Execute git command for every repo

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      COMMAND
    </td>
    <td>
      The command will be executed
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --workspace
    </td>
    <td>
      use the workspace config file
    </td>
  </tr>
</table>

## mbox git hooks

```
$ mbox git hooks
```
Show/Set the git hooks for workspace

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --disable
    </td>
    <td>
      Enable workspace hooks
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --enable
    </td>
    <td>
      Enable workspace hooks
    </td>
  </tr>
</table>

## mbox git-sheet fetch

```
$ mbox git-sheet fetch
```
Perform git fetch for every repo

## mbox git-sheet pull

```
$ mbox git-sheet pull
```
Perform git pull for every repo

## mbox git-sheet push

```
$ mbox git-sheet push
```
Perform git push for every repo

## mbox git-sheet status

```
$ mbox git-sheet status
```
Show git status for every repo

## mbox go

```
$ mbox go [NAME]
```
Quckly open workspace.

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Specific workspace file to open
    </td>
  </tr>
</table>

## mbox init

```
$ mbox init [PLUGIN_GROUP [...]]
```
Init Workspace

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      PLUGIN_GROUP
    </td>
    <td>
      [Optional] A plugin set. Available: android/flutter/ios/macos/plugin/ruby
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --name
    </td>
    <td>
      To set the new MBox workspace (folder) name.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --plugin
    </td>
    <td>
      Config the plugin. It could be used many times to config more plugins.
    </td>
  </tr>
</table>

## mbox kerberos destroy

```
$ mbox kerberos destroy [USEREMAIL]
```
Detroy all tickets

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      USEREMAIL
    </td>
    <td>
      [Optional] User Name, Defaults: james.zhan@bytedance.com
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --keychain
    </td>
    <td>
      Remove password from keychain
    </td>
  </tr>
</table>

## mbox kerberos init

```
$ mbox kerberos init [USEREMAIL]
```
Acquire initial tickets

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      USEREMAIL
    </td>
    <td>
      [Optional] User Name, Defaults: james.zhan@bytedance.com
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --password
    </td>
    <td>
      User Password
    </td>
  </tr>
</table>

## mbox kerberos list

```
$ mbox kerberos list [EMAIL]
```
List Kerberos credentials

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      EMAIL
    </td>
    <td>
      [Optional] User Email
    </td>
  </tr>
</table>

## mbox kerberos renew

```
$ mbox kerberos renew [USEREMAIL]
```
Acquire renew tickets

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      USEREMAIL
    </td>
    <td>
      [Optional] User Name, Defaults: james.zhan@bytedance.com
    </td>
  </tr>
</table>

## mbox login

```
$ mbox login [TIMEOUT]
```
Login MBox

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      TIMEOUT
    </td>
    <td>
      [Optional] Maximum waiting time for Login
    </td>
  </tr>
</table>

## mbox logout

```
$ mbox logout
```
Logout MBox

## mbox merge

```
$ mbox merge [NAME]
```
Merge `other feature`/`target branch` into current feature

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Merge the Feature into current feature
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --no-repo
    </td>
    <td>
      Exclude a repo, use this option multiple times to exclude multiple repos.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --repo
    </td>
    <td>
      Specify a repo, use this option multiple times to specify multiple repos.
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --dry-run
    </td>
    <td>
      Do everything except actually merge.
    </td>
  </tr>
</table>

## mbox new

```
$ mbox new NAME [BRANCH]
```
Create a project in workspace

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      Project Name
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      BRANCH
    </td>
    <td>
      [Optional] Create the branch
    </td>
  </tr>
</table>

## mbox open

```
$ mbox open [PATHS [...]]
```
Open specific path in MBox Environment

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      PATHS
    </td>
    <td>
      [Optional] Specific Path
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --logdir
    </td>
    <td>
      Open log folder
    </td>
  </tr>
</table>

## mbox plugin build

```
$ mbox plugin build [NAME [...]]
```
Build the development plugin(s)

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Plugin Names, otherwise will release all plugins.
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --output-dir
    </td>
    <td>
      The directory for the output
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --stage
    </td>
    <td>
      The build stage Avaliable: Launcher/Resource/Ruby/Electron/Native
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --clean
    </td>
    <td>
      Clean output directory. Defaults: YES if no stage options.
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --force
    </td>
    <td>
      Force release exists version
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --test
    </td>
    <td>
      Run the unit test. Defaults: YES
    </td>
  </tr>
</table>

## mbox plugin dev

```
$ mbox plugin dev TEMPLATE [NAME]
```
Use a template to develop a MBox Plugin

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      TEMPLATE
    </td>
    <td>
      Plugin Template. Avaliable: Launcher/Resource/Ruby/Electron/Native
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Plugin Name (eg: MBoxCore)
    </td>
  </tr>
</table>

## mbox plugin disable

```
$ mbox plugin disable [NAME [...]]
```
Disable plugins by name.

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Plugin names
    </td>
  </tr>
</table>

## mbox plugin enable

```
$ mbox plugin enable [NAME [...]]
```
Enable plugins by name.

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Plugin names
    </td>
  </tr>
</table>

## mbox plugin install

```
$ mbox plugin install [NAME [...]]
```
Install Plugin

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Names of Plugins. Empty(Default) indicate that install all plugins needed.
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --update
    </td>
    <td>
      Update plugin if needed.
    </td>
  </tr>
</table>

## mbox plugin launch

```
$ mbox plugin launch [NAME [...]]
```
Run a plugin launcher

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Launcher names
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --role
    </td>
    <td>
      Set current role, defaults to environment variable `MBOX_ROLES`
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --script
    </td>
    <td>
      Run the script name Avaliable: check/install/uninstall/upgrade
    </td>
  </tr>
</table>

## mbox plugin list

```
$ mbox plugin list
```
List all plugins

## mbox plugin next-version

```
$ mbox plugin next-version [NEW-VERSION]
```
Increments the version numbers

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NEW-VERSION
    </td>
    <td>
      [Optional] Set a version instead of Increments.
    </td>
  </tr>
</table>

## mbox plugin release

```
$ mbox plugin release [NAME [...]]
```
Release a/some development plugin(s)

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Plugin Names, otherwise will release all plugins.
    </td>
  </tr>
</table>

## mbox plugin search

```
$ mbox plugin search NAME
```
Search plugins on the Plugin Market

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      Plugin name
    </td>
  </tr>
</table>

## mbox plugin test

```
$ mbox plugin test [NAME [...]]
```
Test the native plugin(s)

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Plugin Names, otherwise will test all plugins.
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --file
    </td>
    <td>
      Only run specific test file
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --method
    </td>
    <td>
      Only run specific test method
    </td>
  </tr>
</table>

## mbox plugin uninstall

```
$ mbox plugin uninstall [NAME [...]]
```
Uninstall Plugins

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Names of Plugins
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --all
    </td>
    <td>
      Uninstall all plugins in user directory
    </td>
  </tr>
</table>

## mbox pod

```
$ mbox pod
```
Redirect to Bundler with MBox environment

#### Example:
```bash
# Install gems with bundler
$ mbox bundle install
```


## mbox product add

```
$ mbox product add [NAME] [PLATFORM]
```
Add Product

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Product Name or AppID or URL
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      PLATFORM
    </td>
    <td>
      [Optional] Platform Name Avaliable: iOS/Android/Flutter
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --json
    </td>
    <td>
      Product information string
    </td>
  </tr>
</table>

## mbox product remove

```
$ mbox product remove [NAME]
```
Remove Product

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Product Name or AppId
    </td>
  </tr>
</table>

## mbox product sync

```
$ mbox product sync [NAME]
```
Sync Product

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Product Name or AppId
    </td>
  </tr>
</table>

## mbox product update

```
$ mbox product update [NAME]
```
Update Product, it will redeploy dynamic repository if exists.

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Product Name or AppID
    </td>
  </tr>
</table>

## mbox remove

```
$ mbox remove NAME [...]
```
Remove project from workspace

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      Repo Name
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --all
    </td>
    <td>
      Remove all repos
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --force
    </td>
    <td>
      Force remove repo if modified
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --include-repo
    </td>
    <td>
      Remove repo from `.mbox/repos`
    </td>
  </tr>
</table>

## mbox repo search

```
$ mbox repo search NAME
```
Manage Repos

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      The name to search
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --only-search-remote
    </td>
    <td>
      Search repo on remote
    </td>
  </tr>
</table>

## mbox setup

```
$ mbox setup
```
Setup Command Line Tool

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --bin-dir
    </td>
    <td>
      Output the executable bin to specific directory. Default value: `/usr/local/bin`
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --zsh
    </td>
    <td>
      Install autocompletion support for zsh
    </td>
  </tr>
</table>

## mbox status

```
$ mbox status [NAME]
```
Show Status

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      NAME
    </td>
    <td>
      [Optional] Show Other Feature
    </td>
  </tr>
</table>

#### Options:
<table>
  <tr>
    <td style="white-space: nowrap">
      --only
    </td>
    <td>
      Only show information. Avaliable: root/feature/repos/dependencies/containers/products
    </td>
  </tr>
</table>

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --sync
    </td>
    <td>
      Sync config and repos
    </td>
  </tr>
</table>

## mbox stree

```
$ mbox stree [PATHS [...]]
```
Quckly open git repository in the SourceTree app.

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      PATHS
    </td>
    <td>
      [Optional] Specific Path
    </td>
  </tr>
</table>

## mbox tower

```
$ mbox tower [PATHS [...]]
```
Quckly open git repository in the Tower app.

#### Arguments:
<table>
  <tr>
    <td style="white-space: nowrap">
      PATHS
    </td>
    <td>
      [Optional] Specific Path
    </td>
  </tr>
</table>

## mbox update

```
$ mbox update
```
Upgrade MBox Application

#### Flags:
<table>
  <tr>
    <td style="white-space: nowrap">
      --beta
    </td>
    <td>
      Upgrade to the beta version
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --force
    </td>
    <td>
      Force download the latest version
    </td>
  </tr>
  <tr>
    <td style="white-space: nowrap">
      --stable
    </td>
    <td>
      Upgrade to the stable version
    </td>
  </tr>
</table>

## mbox yarn repo

```
$ mbox yarn repo
```
Operation for a single repo.

## mbox yarn repos info

```
$ mbox yarn repos info
```
Show package information about your repos.

