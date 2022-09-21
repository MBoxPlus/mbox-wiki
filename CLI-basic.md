# CLI 基础命令介绍

MBox 提供了强大的命令行处理能力，包括参数解析、透传，同时为了减少用户的学习成本，尽可能的模仿其他工具的风格。

## 命令行参数

`MBoxCore` 是 MBox 的内核，提供了一些基础命令，在任意非 Workspace 的目录下执行（因为在 Workspace 下可能会加载很多 Workspace 的插件导致输出不一样）：

```shell
$ mbox --help
Usage:

    $ mbox

Commands:

    + config       Get/Set Default Configuration
    + doc          Output all commands
    + env          Show MBox Environment
    + init         Init Workspace
    + open         Open specific path in MBox Environment
    + plugin       Manage Plugins
    + setup        Setup Command Line Tool

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

MBox 采用大众化的命令行风格，在任何时候使用 `-h`/`--help` 能显示该命令的帮助信息，同时，在不同的命令下，可以显示不同命令的帮助信息。

在帮助信息中，显示几类板块：
- `Commands`: 介绍该命令下有哪些子命令，例如 `mbox config`、`mbox init` 等
- `Options`: 介绍该命令能接收哪些 Option 参数，Option 参数一般后面会跟着参数值，例如 `--api json`，也可以用 `--api=json`。如果参数值带空格，注意用引号或者转义符。`Option` 可以使用多次，达到多个值的效果，例如 `--path=/a --path=/b`.
- `Flags`: 介绍该命令能接收哪些 Flag。Flag 是特殊的 `Option`，不带参数值的，只有 `true`/`false` 两种，即有 Flag 和没有 Flag。等效于 `Option` 传了一个 `true`/`false` 的值。Flag 可以使用 `--no` 前缀做反向标记。例如 `--launcher` 代表执行的时候做环境检测，默认是开，可以使用 `--no-launcher` 强制关闭该参数，也可以使用 `--launcher=false`。


## 环境变量

MBox 为了方便一些自动化脚本的编写，也支持从环境变量中取参数。环境变量名的格式为 `MBOX_ + 大写参数名`。

例如：
- `MBOX_VERBOSE=1` 等效于使用 `--verbose`
- `MBOX_API=json` 等效于使用 `--api=json`

需要注意：

- 环境变量只能用在 `Option` 和 `Flag` 参数上，不能使用在 `Argument` 类型参数上
- `Flag` 的 `Bool` 类型支持 `yes`/`no`/`true`/`false`/`0`/`1`，不区分大小写
- `Option` 的值如果需要多个，可以使用英文逗号分隔，且使用复数的变量名，例如 `mbox xx --path /a --path /b` 可以使用 `export MBOX_PATHS=/a,/b`

## 插件化

由于 MBox 是插件化开发，用户能看到什么命令，取决于激活了哪些插件。

`MBoxWorkspace` 插件是 MBox 最常用的插件，很多插件都是基于它扩展，它本身也提供了 Repository/Feature/Git 等命令。当该插件未激活的时候，是无法查看到这些命令的，因为这个插件包整个都未加载起来，是没有向 MBox 注册命令的时机。

例如，当你有一个 Workspace 的时候，在该 Workspace 及其子目录下，是可以正常使用 `mbox add` 等命令。当你的终端路径不在 Workspace 下，输入 `mbox add` 等命令，是会提示找不到该命令的！

```shell
$ cd ~/A # 普通的目录
$ mbox add git@github.com:xx/xx.git
Not found command `add` (~/A)

$ cd ../MBoxTests # Workspace 目录
$ mbox add git@github.com:xx/xx.git
# 正常执行 add 命令
```

例如，当你的项目是 iOS 项目，但是，项目中没声明激活 `MBoxCocoapods` 插件，在 初始化 Workspace 的时候，没有声明 `ios`（`mbox init ios`），那 MBox 是不会默认激活 `MBoxCocoapods` 插件的。这时候在运行 `mbox pod` 的时候也是会提示 `Not found command 'pod'`。
