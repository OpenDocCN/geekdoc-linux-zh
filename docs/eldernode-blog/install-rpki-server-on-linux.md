# 在 Linux - Eldernode 博客上安装 RPKI 服务器的教程

> 原文：<https://blog.eldernode.com/install-rpki-server-on-linux/>

![Tutorial Install RPKI Server on Linux](img/074992a1a289dd7595f53718b743ce57.png)

RPKI 服务器是建立 BGP 协议安全性的方法之一。NLNetLabs 拥有的 Routinator 是最好的 RPKI 服务器之一。在本文中，我们将一步步教你如何在 Linux 上安装 RPKI 服务器。另外，如果你想购买一个 [**Linux VPS**](https://eldernode.com/linux-vps/) 主机，你可以访问 [Eldernode](https://eldernode.com/) 中的软件包。

## **如何在 Linux 上安装 RPKI 服务器【Ubuntu，Debian】**

在 [linux](https://blog.eldernode.com/tag/linux/) 上安装 RPKI 服务器之前，先更新 linux 软件包。

```
sudo apt update
```

```
sudo apt upgrade
```

然后安装以下软件包:

```
sudo apt-get install curl wget gcc rsync
```

由于 routinator 是用 Rust 语言编程的，我们需要在服务器上安装这种语言:

```
curl https://sh.rustup.rs -sSf | sh
```

注意:显示新页面时，键入 1，然后按 Enter。

然后将路径环境更改为输出中显示的路径。

```
source $HOME/.cargo/env
```

此外，确保通过以下命令检查服务器上的 gcc:

```
gcc --version
```

## **如何在 Linux 上安装 Routinator**

在 linux 服务器上使用 Rust 语言包安装 Routinator 包:

```
cargo install routinator
```

然后使用以下命令运行 Routinator:

```
routinator init --accept-arin-rpa
```

使用以下命令从目标服务器下载所有 roa:

```
routinator -v vrps
```

要启用 Routinator web 环境，只需在终端中运行以下命令。

```
routinator server --http ip_server:8323
```

您可以通过在浏览器中打开 ip_server:8323 来查看用户界面。

## 结论

恭喜你，你可以在 Linux ubuntu 或者 debian 服务器上安装 RPKI 服务器了。如果你对此有疑问或问题，可以在评论中提问。