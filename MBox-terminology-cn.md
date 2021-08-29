# Concepts and Terminology

**简体中文** | [English](https://github.com/MBoxPlus/mbox/wiki/MBox-terminology) 

## Workspace

使用 MBox 创建的工作空间，用于维护代码仓库和研发环境。

## Feature

 workspace 内的最小工作单元，每个 feature 都是一个由代码仓库和研发环境构成的沙盒环境，用户可在 workspace 内创建多个 feature 并自由切换。


## Free Mode 和 Feature Mode

**Free Mode** 是新建的 workspace 的默认状态，该状态下，用户可以任意切换仓库的 HEAD 游标，使用 `mbox feature free` 可进入该模式。

**Feature Mode** 当用户切换到任意一个 feature 时，即进入 Feature Mode，该状态下，所有仓库的 HEAD 游标都处于同名的 feature 分支上，使用 `mbox feature start [FEATURE NAME]` 可进入该模式。

## Container

用于构建项目的抽象集合，包括工程和依赖配置，一个仓库可以存在多种类型的 Container ，比如在 iOS 主仓中往往存在 Bundler 和 CocoaPods 两个容器，分别用于管理构建工具链和 iOS 应用。
