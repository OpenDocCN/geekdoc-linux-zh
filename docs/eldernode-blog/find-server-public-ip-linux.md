# 如何在 Linux 终端- Eldernode 中找到服务器公共 IP 地址

> 原文：<https://blog.eldernode.com/find-server-public-ip-linux/>

![How to find server public IP Address in Linux terminal](img/44d7cb18c958b78541829b2ab295de09.png)

一个 Linux 用户需要知道一些 Linux 的技巧。在本文中，您将学习如何在 Linux 终端中找到服务器公共 IP 地址。

如您所知，在计算机网络中，IP 地址是永久或临时分配给每台连接到使用互联网协议进行通信的网络的设备的数字标识符。识别网络或在网络上托管是它的功能。我们使用两个当前版本的 IP 地址: **IPv4 和 IPv6** 。这些地址可以是私有的，也可以是公有的。

此外，有两种方式可以分配主机，静态或动态 IP 地址，这将根据网络配置来选择。

## 如何在 Linux 终端找到服务器公共 IP 地址

让我们来看看这 4 种帮助用户找到他们的服务器公共 IP 地址的方法。

您可能对以下内容感兴趣:

[用 netplan](https://eldernode.com/set-ip-static-on-ubuntu-20-04-lts-server-with-netplan/) 在 Ubuntu 20.04 LTS 服务器上设置 IP 静态

[如何给 Ubuntu 添加第二个 IP](https://eldernode.com/add-second-ip-to-ubuntu/)

### 1。使用挖掘工具

为了探测 DNS 名称服务器，您需要一个实用程序。所以使用**opendns.com**解析器找到你的公共 **IP 地址**。

```
dig +short myip.opendns.com @resolver1.opendns.com    **120.88.41.175** 
```

### 2。使用主机实用程序

为了执行 DNS 查找，您需要使用 host 命令。运行以下命令，在 **Linux** 终端中显示您系统的公共 IP 地址。

```
host myip.opendns.com resolver1.opendns.com | grep "myip.opendns.com has" | awk '{print $4}'    **120.88.41.175**
```

请注意:在剩下的两种查找您的 IP 地址的方法中，我们将在命令行中显示第三方网站。

### 3。使用 wget 命令行下载器

您可以使用 [Wget](https://en.wikipedia.org/wiki/Wget) ，它是一个强大的命令行下载程序，支持各种协议，比如 HTTP、HTTPS、FTP 等等。此外，**还可以通过第三方网站找到**你的公共 IP 地址。

```
wget -qO- http://ipecho.net/plain | xargs echo  wget -qO - icanhazip.com    **120.78.58.115** 
```

### 4。使用 cURL 命令行下载器

使用任何支持的协议(HTTP、HTTPS、文件、FTP、FTPS 等)从服务器上传或下载文件的最流行的工具之一是 **curl** 。所以使用下面的命令来查看您的**公共 IP 地址**。

```
curl ifconfig.co  curl ifconfig.me  curl icanhazip.com    **120.78.58.115** 
```

**万一**需要多读，就跟着 [Linux 招数](https://eldernode.com/tag/linux-tricks/) 。

亲爱的用户，我们希望你能喜欢如何在 Linux 终端中找到服务器公共 IP 地址的教程，你可以在评论区提出关于这次培训的问题，或者解决[elder node training](https://eldernode.com/blog/)领域的其他问题，参考 [提问页面](https://eldernode.com/ask) 部分并在其中提出你的问题。