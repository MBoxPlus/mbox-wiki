# MBox 配置

## 一般配置文件

MBox 为了覆盖多种场景和支持用户自定义，有较多的配置文件：

1. `~/.mbox/config.json` 全局配置文件
1. `[WORKSPACE]/.mboxconfig` Workspace 根目录配置文件
1. `[REPOSITORY]/.mboxconfig` 仓库根目录配置，每个仓库都可能会有一个配置文件

MBox 的配置文件采用 JSON 格式，会按顺序加载配置文件，并覆盖之前加载的配置值。

仓库根目录的配置 不仅仅可以描述当前仓库的信息，例如 容器的配置、组件的配置，也可以描述 MBox 的配置，将会合并覆盖 Workspace 配置和 全局 配置。

MBox 提供了一个命令查询修改配置：

```shell
# 显示所有配置内容（叠加 Global、Workspace、Repository）
$ mbox config

# 显示全局配置内容
$ mbox config --global

# 显示 Workplace 配置内容
$ mbox config --workspace

# 不支持单独显示某个仓库的配置内容

# 设置配置到 Workspace 配置
$ mbox config workspace.use_worktree true

# 设置配置到全局配置
$ mbox config workspace.use_worktree true --global
```

## Shell 环境脚本

在 Shell 环境配置过程中，我们发现，配置环境变量的概率远大于其他配置，但是，无论是 `.mboxrc` 还是 `.zshrc` 都是不容易读写的格式，因此 MBox 独立出了 `~/.mbox/environment.conf` 文件，专门存储环境变量的配置。

可以使用 `mbox config --rc` 命令进行读写环境变量配置。
