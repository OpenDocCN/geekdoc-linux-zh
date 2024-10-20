# 学习在 Linux 上使用 Fping 工具-教程 Linux | Eldernode

> 原文：<https://blog.eldernode.com/install-and-work-fping-tools-on-linux/>

![Learning to work with Fping tools on Linux](img/3875c4f84b653072d8a05bc033009295.png)

如何在 [Linux](https://eldernode.com/tag/linux/) 上使用 Fping 工具？

当然，现在没有人不熟悉 Ping 命令，也没有人不知道，使用这个命令，ICMP 数据包可以发送到网络上的另一个设备，并被告知其状态。在这篇来自 Linux 教程系列的文章中，我们将向您介绍一个叫做 fping 的有吸引力的工具，这样您就可以利用它的实际功能。加入我们，了解如何在 linux 上使用 Fping 工具。

**什么是 Fping？**

Fping 是一个小工具，可以单独安装在 Linux 上，用它来锁定网络中的设备。但是这个工具的重要之处在于，它与 Ping 命令截然不同。它能够 ping 通网络上的多个设备，允许我们同时 ping 通网络内的所有系统或不同的 IP 地址。

现在就加入我们，学习如何在 Linux 上使用 Fping 工具，当然还要安装它

### 学习在 Linux 上使用 Fping 工具

**Fping 安装**

您可以使用以下命令在不同版本的 Linux 中安装 fping:

在 CentOS 和 Redhat 上安装 fping

```
yum install fping 
```

在 [Debian](https://www.debian.org/) 和 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) 上安装 fping

```
apt install fping 
```

在 Fedora 中安装 fping

```
dnf install fping 
```

在 Arch Linux 中安装 fping

```
pacman -S fping 
```

因此，您将能够在您的 Linux 上安装 fping。

安装后，为了确保正确的安装状态，可以输入以下命令来显示服务器上已安装的 fping 版本。

```
fping -v 
```

上述命令的输出如下所示。

```
Output    fping: Version 4.0  fping: comments to [[email protected]](/cdn-cgi/l/email-protection) 
```

**注**:如果输出正确显示工具版本，则说明 fping 安装正确。

到目前为止，您已经成功地在您的 Linux 上安装了 fping 接下来，我们将教你如何在 Linux 中使用它。

使用 Fping 命令

使用 fping 命令同时测试和检查多个不同 IP 地址的最简单方法如下。

```
fping 40.116.66.139 173.194.35.35 98.139.183.24 
```

输入上述命令后，该命令的输出将如下所示。

```
Output    40.116.66.139 is alive  173.194.35.35 is unreachable  98.139.183.24 is unreachable 
```

要使用 fping 检查和测试一系列 IP 地址，您可以按如下方式输入 fping 命令。

```
fping -s -g 192.168.0.1 192.168.0.9 
```

输入上述命令后，您将得到与以下语句相同的输出。

```
192.168.0.1 is alive  192.168.0.2 is alive  ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.3  ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.3  ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.3  ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.4  192.168.0.3 is unreachable  192.168.0.4 is unreachable    8 9 targets  2 alive  2 unreachable  0 unknown addresses    4 timeouts (waiting for response)  9 ICMP Echos sent  2 ICMP Echo Replies received  2 other ICMP received    0.10 ms (min round trip time)  0.21 ms (avg round trip time)  0.32 ms (max round trip time)  4.295 sec (elapsed real time) 
```

要完全 ping 通子网，您可以输入如下所示的 fping 命令。

```
fping -g -r 1 192.168.0.0/24 
```

请注意，在上面的命令中，使用了带数字 1 的-r 参数，这意味着整个子网只需在 ping 命令中出现一次。

最后，fping 的另一个有趣的特性是从 TXT 文件中读取 IP 地址的能力，这使您能够从所有需要的 IP 地址创建一个文件，并在任何时候调用它。

```
fping < IPs.txt 
```

在上面的命令中，我们已经创建了一个名为 IPs.txt 的文件。

我们希望你喜欢在 Linux 上使用 Fping 工具的教程。

如有疑问或问题，可向[提问系统](https://eldernode.com/ask/)咨询，提供指导。