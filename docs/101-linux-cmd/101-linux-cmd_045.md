# `wget` 命令

`wget` 命令用于从互联网下载文件。它支持使用 HTTP、HTTPS 和 FTP 协议下载文件。它允许您一次性下载多个文件，后台下载，恢复下载，限制带宽，镜像网站，等等。

## 语法

`wget` 的语法要求您定义下载选项以及要下载的文件的 URL。

```sh
      $ wget [options] [URL] 

```

### 示例

在此示例中，我们将从不同的来源下载 Ubuntu 20.04 桌面 iso 文件。转到您的终端或打开一个新的终端，并输入下面的 `wget`。这将开始下载。下载可能需要几分钟才能完成。

1.  开始常规下载

```sh
      wget https://releases.ubuntu.com/20.04/ubuntu-20.04.3-desktop-amd64.iso 

```

1.  您可以使用 `-c` 选项恢复下载

```sh
      wget -c https://mirrors.piconets.webwerks.in/ubuntu-mirror/ubuntu-releases/20.04.3/ubuntu-20.04.3-desktop-amd64.iso 

```

1.  要在后台下载，请使用 `-b` 选项

```sh
      wget -b https://mirrors.piconets.webwerks.in/ubuntu-mirror/ubuntu-releases/20.04.3/ubuntu-20.04.3-desktop-amd64.iso 

```

## 更多选项

除了下载之外，`wget` 还提供了许多其他功能，例如下载多个文件、后台下载、限制下载带宽和恢复中断的下载。查看其 man 页面中的所有 `wget` 选项。

```sh
      man wget 

```

### 其他标志及其功能

| **短标志** | **描述** |
| --- | --- |
| `-v` | 打印系统上可用的 `wget` 版本 |
| `-h` | 打印显示所有可能选项的帮助信息 |
| `-b` | 此选项用于在启动时将进程发送到后台。 |
| `-t` | 此选项用于设置重试次数为指定的次数 |
| `-c` | 此选项用于恢复部分下载的文件 |
