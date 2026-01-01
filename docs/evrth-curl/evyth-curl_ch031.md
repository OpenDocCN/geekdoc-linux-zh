# macOS

macOS 已经将 curl 工具捆绑在操作系统中多年。如果你想升级到 curl 项目提供的最新版本，我们建议安装 [homebrew](https://brew.sh/)（一个 macOS 软件包管理器），然后从那里安装 curl 包：

```sh
brew install curl
```

注意，在安装 curl 时，brew 不会在默认的 homebrew 文件夹中创建一个 `curl` 符号链接，以避免与 macOS 版本的 curl 发生冲突。

运行以下命令以使 brew curl 成为你的 shell 中的默认版本：

```sh
echo 'export PATH="$(brew --prefix)/opt/curl/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

## 获取 macOS 的 libcurl

当你按照上述描述使用 homebrew 安装 `curl` 工具时，它也会一起安装 libcurl 及其相关的头文件。

libcurl 也随着 macOS 本身安装，并且始终可用。如果你从 Apple 安装了开发环境 `XCode`，你可以直接使用 libcurl，无需安装任何额外的东西，因为 curl 的包含文件已经包含在其中。
