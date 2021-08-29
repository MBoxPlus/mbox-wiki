# MBox Terminology

[简体中文](https://github.com/MBoxPlus/mbox/wiki/MBox-terminology-cn) | **English**

## Workspace

A directory created by MBox，which is used to maintain git repositories and development environments.

## Feature

A feature is the minimum unit in a workspace. Each feature is sandbox that contains git repositories and development environments.  MBox let users create multiple features and switch between them.


## Free Mode 和 Feature Mode

**Free Mode** is the default state of the workspace. In this state, the HEAD pointer of all the git repositories is allowed to refer to any branch or tag. Use command `mbox feature free` to switch to Free Mode.

**Feature Mode** When the user switches to any one of features, it will be in Feature Mode. In this state, the HEAD pointers of all the git  repositories refer to the feature branch with the same name. Use the `mbox feature start [FEATURE NAME]` to switch between features.

## Container

An abstract collection for building projects, including project and dependency configurations. A repository may have more than one kind of Container, such as Bundler and CocoaPods, which are used to manage the build toolchain and iOS application.
