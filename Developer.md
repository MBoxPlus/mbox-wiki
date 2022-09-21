
### 开发自己的依赖管理工具

与开发标准插件类似，依赖 `MBoxContainer` 和 `MBoxDependencyManager`，需要 Hook 三个方法，以 `MBoxRuby` 添加 `Bundler` 依赖为例：

```swift
// 添加依赖管理工具 Bundler
extension MBDependencyTool {
    public static let Bundler = MBDependencyTool("Bundler")

    @_dynamicReplacement(for: allTools)
    public static var ruby_allTools: [MBDependencyTool] {
        var tools = self.allTools
        tools.insert(.Bundler, at: 0)
        return tools
    }
}


extension MBWorkRepo {
    // 添加对 Bundler 容器的识别
    @_dynamicReplacement(for: fetchContainers())
    open func ruby_fetchContainers() -> [MBContainer] {
        var value = self.fetchContainers()
        if self.path.appending(pathComponent: "Gemfile").isExists {
            value.append(MBContainer(name: self.name, tool: .Bundler))
        }
        return value
    }

    // 添加对 Bundler 组件的识别
    @_dynamicReplacement(for: resolveDependencyNames())
    open func ruby_resolveDependencyNames() -> [(tool: MBDependencyTool, name: String)] {
        var names = self.resolveDependencyNames()
        // allGemspecPaths 方法获取仓库下所有 gemspec 文件
        let data = self.allGemspecPaths().map {
            (
                tool: MBDependencyTool.Bundler,
                name: $0.lastPathComponent.fileName
            )
        }
        names.append(contentsOf: data)
        return names
    }
}
```
