# 如何在 Linux 中使用 Fping——在 Linux 上安装 Fping

> 原文：<https://blog.eldernode.com/work-with-fping-in-linux/>

![How to work with Fping in Linux](img/b55a6c698dcce0d928e96028eca1d003.png)

在本教程中，我们将学习如何在 Linux 中使用 Fping。如今，大多数用户都熟悉 Ping 命令，即 [ICMP](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol) 数据包可以发送到网络上的另一个设备，并且可以通知其状态。我们将向您介绍一个名为 fping 的有趣工具，以便您可以利用它的实际功能。

## 如何在 Linux 中使用 Fping

### 什么是 Fping？

Fping 是一个小工具，可以单独安装在 Linux 上，用它来锁定网络中的设备。但是这个工具的重要之处在于它能够 ping 通网络上的多个设备，这使我们能够同时 Ping 通网络内的所有系统或不同的 IP 地址。

### 如何安装 Fping？

要在版本中安装 fping，请使用以下命令访问 [Linux](https://eldernode.com/tag/linux/) :

**在 CentOS 和 Redhat 上安装 fping**

```
yum install fping
```

**在 Debian 和 Ubuntu 中安装 fping**

注意:root 用户可以直接运行下面的命令，但是如果没有 root 用户权限，在执行命令之前必须添加 **sudo** 。

```
apt install fping
```

**在 Fedora 中安装 Fping】**

```
dnf install fping
```

**在 Arch Linux 中安装 fping**

```
pacman -S fping
```

### 安装后应该做什么？

这样你就可以安装并决定你的 Linux 了。

安装后，您可以输入以下命令来确保 fping 的安装版本显示在您的服务器上，以确保正确的安装状态。

```
fping -v 
```

上述命令的输出如下。

```
Output    fping: Version 4.0  fping: comments to [[email protected]](/cdn-cgi/l/email-protection)
```

注:如果输出正确显示工具版本，则说明 fping 安装正确。

到目前为止，你已经成功地在你的 Linux 上安装了 fping，接下来，我们将教你如何在 Linux 上使用它。

### 如何使用 Fping 命令

使用 fping 命令同时测试和检查多个不同 IP 地址的最简单方法如下。

```
fping 50.116.66.139 173.194.35.35 98.139.183.24
```

输入上述命令后，该命令的输出将如下所示。

```
Output    50.116.66.139 is alive  173.194.35.35 is unreachable  98.139.183.24 is unreachable
```

要使用 fping 检查和测试一系列 IP 地址，您可以按如下方式输入 fping 命令。

```
fping -s -g 192.168.0.1 192.168.0.9
```

输入上述命令后，您将得到与以下语句相同的输出。

```
192.168.0.1 is alive  192.168.0.2 is alive
```

```
ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.3  ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.3  ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.3  ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.4  192.168.0.3 is unreachable  192.168.0.4 is unreachable    8 9 targets  2 alive  2 unreachable  0 unknown addresses    4 timeouts (waiting for response)  9 ICMP Echos sent  2 ICMP Echo Replies received  2 other ICMP received    0.10 ms (min round trip time)  0.21 ms (avg round trip time)  0.32 ms (max round trip time)  4,295 sec (elapsed real time)  To ping a subnet completely, you will be able to enter the fping command as shown below.
```

fping -g -r 1 192.168.0.0/24

注意:在上面的命令中，使用了编号为 1 的 **-r** 参数，这意味着整个命令只需要 ping 一次。

最后，fping 的另一个有趣的特性是从 TXT 文件中读取 IP 地址的能力，这使您能够从所有需要的 IP 地址创建一个文件，并在您需要时回调它。

```
fping <IPs.txt
```

在上面的命令中，我们已经创建了一个名为 IPs.txt 的文件。

这样，您可以使用 fping 工具同时 ping 整个网络、一个 IP 范围或多个不同的 IP 地址，并了解它们的状态。

亲爱的用户，我们希望你会喜欢这个如何在 Linux 中使用 Fping 的教程，你可以在评论区提出关于这个培训的问题，或者解决 [Eldernode 培训](https://eldernode.com/blog/)领域的其他问题，请参考[提问页面](https://eldernode.com/ask)部分，并尽快提出你的问题。腾出时间给其他用户和专家来回答你的问题。