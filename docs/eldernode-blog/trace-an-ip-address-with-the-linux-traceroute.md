# 如何使用 Linux Traceroute 命令跟踪 IP 地址

> 原文：<https://blog.eldernode.com/trace-an-ip-address-with-the-linux-traceroute/>

![How to Trace an IP Address with the Linux Traceroute Command](img/13fc492f38703e3c363f113b31e4593a.png)

如果在互联网上上传站点或连接到系统时遇到问题，可以使用 Traceroute 命令找出连接问题所在。此外，该命令可以显示从您的计算机到所需网站所在的 web 服务器的通信路线图。本文将教您如何使用 Linux Traceroute 命令跟踪 IP 地址。如果你想购买自己的 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务器，可以查看 [Eldernode](https://eldernode.com/) 网站上提供的软件包。

## **什么是 Traceroute 命令？**

Traceroute 是一种命令行工具，可用于 Windows、Linux 和其他操作系统。它是解决和理解连接问题的有用工具。此命令行工具显示在本地计算机和目标 IP 地址或域之间传输数据包所需的时间。事实上，借助该工具，可以识别网络中的路由问题，因为该工具可以显示网络路径中的下一台路由器。

在 [Linux 培训](https://blog.eldernode.com/tag/linux/)系列的这篇文章的后续文章中，您将学习如何使用 Linux Traceroute 命令跟踪 IP 地址。

### **教程在 Linux 上安装 Traceroute 命令**

Traceroute 可用于大多数分发存储库。在这一步中，我们将解释如何使用软件包管理器在 Linux 系统上安装 Traceroute 命令。

您可以使用下面的命令在 Ubuntu/Debian 上安装 Traceroute:

```
sudo apt install traceroute
```

要在 Fedora 上安装 Traceroute，请运行以下命令:

```
dnf install traceroute
```

OpenSUSE 命令:

```
zypper in traceroute
```

Arch Linux 命令:

```
pacman -S traceroute
```

## **用 Linux Traceroute 命令** 追踪 IP 地址

在本节中，您将学习如何使用 Linux Traceroute 命令跟踪 IP 地址。为此，请按照以下步骤操作:

```
traceroute [options] <hostname or IP> [packet length]
```

***注意:*** 注意不要用 Tracepath 代替 Traceroute。这两个命令适用于 Linux，并且是相似的。Tracepath 对所有用户都可用，并且输出较少的信息。但是 Traceroute 提供了更多的选项，其中一些需要 root 权限。

### **Traceroute 命令的高级选项**

以下是 Linux Traceroute 命令的一些高级选项:

**-n 选项—>**隐藏设备名称:

```
traceroute -n <hostname or IP>
```

**-N 选项—>**查看同时发送的探头数量:

```
traceroute -N <hostname or IP>
```

**-I 选项->**为 ICMP 探测数据包添加:

```
traceroute -I <hostname or IP>
```

**-q 选项->-**改变数据包的数量:

```
traceroute -Inq <number> <hostname or IP>
```

**-d 选项—>**在 Linux 上启用调试:

```
traceroute -d <hostname or IP>
```

**-p 选项->-**定义查询的端口:

```
traceroute -p <hostname or IP>
```

**-P 选项—>**发送指定 IP 协议的数据包:

```
traceroute -P <hostname or IP>
```

就是这样！

## 结论

Traceroute 命令是一种工具，用于显示在本地计算机和目的 IP 地址或域之间传输数据包所需的时间。在本文中，我们教您如何使用 Linux Traceroute 命令跟踪 IP 地址。此外，我们还解释了 Traceroute 命令的一些高级选项。我希望这个教程对你有用。如果您有任何问题或建议，可以在评论区联系我们。