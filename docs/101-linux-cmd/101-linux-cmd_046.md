# `curl`命令

在 Linux 中，curl 是一个功能强大的命令行工具，用于使用广泛的协议（包括 HTTP、HTTPS 和 FTP）从服务器或向服务器传输数据。它通常用于测试 API、下载文件和自动化与 Web 相关的任务。

## `curl`命令的语法：

```sh
      $ curl [options...] <url> 

```

该命令将在终端窗口中打印出 example.com 主页的源代码。

## 常用选项：

curl 有超过 200 个选项！以下是一些最常见和有用的选项。

| 选项 | 长版本 | 描述 |
| --- | --- | --- |
| `-O` | `--remote-name` | 下载文件并使用与远程文件相同的名称保存。 |
| `-o <file>` | `--output <file>` | 将下载的输出保存到特定的文件名。 |
| `-L` | `--location` | 如果服务器报告请求的页面已移动，则跟随重定向。 |
| `-X <METHOD>` | `--request <METHOD>` | 指定要使用的 HTTP 请求方法（例如，POST、PUT、DELETE）。 |
| `-H <header>` | `--header <header>` | 允许您向请求添加自定义 HTTP 头。 |

## 示例：

### 1. 查看网页的源代码

这是 curl 最简单的用法。它将从 URL 获取内容，并将其 HTML 源代码直接打印到您的终端。

```sh
      $ curl example.com 

```

### 2. 下载一个文件

`-O`标志用于下载文件。curl 会使用与远程文件相同的名称将其保存在您的当前目录中。

```sh
      $ curl -O https://github.com/bobbyiliev/101-linux-commands/archive/refs/tags/v1.0.zip 

```

### 3. 下载文件并重命名

使用`-o`标志，您可以指定下载文件的新的名称。

```sh
      $ curl -o linux-commands.zip https://github.com/bobbyiliev/101-linux-commands/archive/refs/tags/v1.0.zip 

```

## 安装：

curl 命令包含在大多数 Linux 发行版中。但是，如果系统默认没有携带 curl，您需要手动安装。要安装 curl，请执行以下命令：

通过执行以下命令来更新系统：

```sh
      $ sudo apt update
$ sudo apt upgrade 

```

现在，通过执行以下命令来安装 curl 实用工具：

```sh
      $ sudo apt install curl 

```

通过执行以下命令来验证安装：

```sh
      $ curl -version 

```

上述命令将显示 curl 命令的安装版本。
