# WSL 使用笔记

## 安装

参考教程：

- [Install WSL](https://docs.microsoft.com/en-us/windows/wsl/install#update-to-wsl-2)
- [microsoft-wsl-install-manual](https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-6---install-your-linux-distribution-of-choice)

默认情况下安装的 Linux 发行版是 Ubuntu。

## 常用命令

1. `wsl -l -v` 或者 `wsl --list --verbose`

    - 检查每个已安装的 Linux 发行版设置的 WSL 版本。
    - 显示有关所有发行版的详细信息。此命令列出每个发行版的名称，发行版所处的状态以及正在运行的版本。默认发行版标以星号。
    - `wsl --list --quiet`
    仅列出发行版名称。此命令对于脚本编写很有用，因为它只会输出你已安装的发行版的名称，而不显示其他信息，如默认发行版、版本等。

2. `wsl --set-default-version <Version#>`

    - 将安装新的 Linux 发行版的默认版本设置为 WSL 1 或 WSL 2，替换`<Version#>`为 `1` 或 `2`。

3. `wsl --set-version <distro name> 2`

    - 将以前安装的 Linux 发行版上从 WSL 1 更新到 WSL 2，`<distro name>`为要更新的 Linux 发行版的名称。例如，`wsl --set-version Ubuntu-20.04 2`，将 `Ubuntu 20.04` 发行版设置为使用 WSL 2。

## 启动和关闭

1. 启动有4种方式：

    - 1）直接在 `开始` 菜单单击启动。
    - 2）`WIN+R`打开运行对话框，输入 `bash` 启动。
    - 3）在 PowerShell/CMD 中输入 `bash` 启动。
    - 4）使用 Windows Terminal 打开 `Ubuntu-20.04.4LTS` 的终端。

2. 关闭2种方式：

    - 在 `开始` 菜单找到 `Ubuntu-20.04.4LTS` 选项，右键选择 `更多-应用设置-终止`。
    - `wsl -t DISTRO-NAME`
      - 终止单个运行的发行版，将 `DISTRO-NAME` 替换为要关闭的发行版的名称，例如，`wsl -t Ubuntu-20.04`。
    - `wsl --shutdown`
      - 立即终止所有正在运行的发行版和 WSL 2 轻量级实用程序虚拟机。
      - 一般来说，支持 WSL 2 发行版的虚拟机是由 WSL 来管理的，因此会在需要时将其打开并在不需要时将其关闭。但也可能存在你希望手动关闭它的情况，此命令允许你通过终止所有发行版并关闭 WSL 2 虚拟机来执行此操作。

## 开发环境配置

1. IDE 配置

    - VScode 安装插件 [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)

2. [Go 环境配置](https://github.com/Harderboy/Go-Notes/blob/main/Go%20%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE.md)
