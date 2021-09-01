# Getting Started for iOS

[简体中文](Getting-Started-iOS-cn) | **English**

## 1. Create Workspace

Init a MBox workspace

```shell
mkdir HelloMBox && cd HelloMBox # Create a directory

mbox init ios # Initialize a workspace
```

## 2. Configuration Container
> `Container` is a git repository for storing all of app project or dependency specification. 

Add `.mboxconfig` file to the root directory of your project.

```JSON
{
    "podfile": "Relative path to Podfile",
    "xcodeproj": "Relative path to xcodeproj",
    "podlock": "Relative path to Podfile.lock"  // optional
}
```

Commit and push this change.

If you don't have your own project, here is a [demo](https://github.com/MBoxPlus/MBoxReposDemo/blob/main/.mboxconfig).

> We strongly recommend you use `Gemfile` to manager the version of `CocoaPods`.

## 3. Add Container

Run this command to add your app's repository to the workspace.

```shell
mbox add [GIT_URL] [BRANCH]
# For Example: mbox add https://github.com/MBoxPlus/MBoxReposDemo.git main
```

## 4. Pod Install

Add `mbox` prefix to the `pod install` command.

```shell
mbox pod install
```

Wait for minutes. (How long depends on how many pods)

## 5. Open Xcode

Use `mbox go` to open Xcode Workspace. You can now build and run your App.

```shell
mbox go
```

## 6. Create Feature

```shell
mbox feature start [feature_name]
# For Example: mbox feature start hello_mbox
```

## 7. Dependent Repository

Before it, you need to run `mbox pod install` again.

```shell
mbox pod install
```

Usually, we have some dependent repositories to develop at the same time.

```shell
mbox add [DEPENDENT_GIT_URL] [TARGET_BRANCH] --checkout-from-commit
# For Example: mbox add https://github.com/SnapKit/SnapKit.git develop --checkout-from-commit
```

> `--checkout-from-commit` Checkout from the commit of the specific version that matches the dependency relationship.

Please check if the `.podspec` file is under the root repo directory. Otherwise, you need to create a `.mboxconfig` file at the root directory.

```JSON
{
    "podspecs": ["Relative path to podspec", ...]
}
```

Run `mbox pod install` again. You will find this repo is under the `Development Pods` group in Xcode.

---

## Resources

[Quick Start Demo](Quick-Start-Demo-iOS)

[CLI Documentation](CLI-documentation)

[MBoxCocoaPods Documentation](https://github.com/MBoxPlus/mbox-cocoapods)