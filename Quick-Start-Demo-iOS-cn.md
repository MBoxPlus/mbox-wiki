# 快速开始 Demo

**简体中文** | [English](Quick-Start-Demo-iOS)

通过下面的步骤，将帮助你快速了解如何使用 MBox

```shell
mkdir HelloMBox && cd HelloMBox # Create a directory

mbox init ios # Initialize a workspace

mbox add https://github.com/MBoxPlus/MBoxReposDemo.git main

mbox add https://github.com/Alamofire/Alamofire.git master

mbox pod install

mbox go # Open Xcode
```

更多请参考 [iOS 项目的接入与使用](Getting-Started-iOS-cn) 来学习如何在你的项目中接入 MBox.

## 资源

[CLI 文档](CLI-documentation)

[MBoxCocoaPods 文档](https://github.com/MBoxPlus/mbox-cocoapods)
