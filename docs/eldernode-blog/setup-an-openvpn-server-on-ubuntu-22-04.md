# 如何在 Ubuntu 22.04 - Eldernode 博客上设置 OpenVPN 服务器

> 原文：<https://blog.eldernode.com/setup-an-openvpn-server-on-ubuntu-22-04/>

![How to Setup an OpenVPN Server on Ubuntu 22.04](img/ab7ef0c6707de03b911df9cec243376f.png)

OpenVpn 是目前作为 Vpn 使用的最流行的协议，你检查的任何 VPN 提供商肯定会在他们的协议列表中有这项服务。甚至一些提供商满足于这个协议，只提供 OpenVPN。加入我们这篇文章，教你如何在 Ubuntu 22.04 上设置 OpenVPN 服务器。如果你想购买一台 **[Ubuntu VPS](https://eldernode.com/ubuntu-vps/)** 服务器，你可以查看 [Eldernode](https://eldernode.com/) 网站上提供的软件包。

## **教程在 Ubuntu 22.04 上安装 OpenVPN 服务器**

### **OpenVPN 简介**

OpenVPN 是一个商业开源软件，允许您实现点对点或站点到站点的 VPN。OpenVPN 是最安全的过滤协议之一，由于其开源的特性而不能被阻塞，并且具有高度的灵活性。OpenVPN 协议可以基于 TCP 协议建立自己的加密隧道以获得最大的可靠性，或者基于 UDP 以提高速度。

在本文的续篇中，在提到 OpenVPN 的一些好处之后，我们将一步一步的教你如何在 [Ubuntu](https://blog.eldernode.com/tag/ubuntu/) 22.04 上设置 OpenVPN 服务器。

### **OpenVPN 优势**

->非常安全

->难以阻挡

->完全控制连接

->完全支持领先隐私

->针对奸商的数据保护

->兼容多种操作系统

## **在 Ubuntu 22.04 上安装 OpenVPN 服务器**

对于没有使用 Linux 和 VPN 经验的新用户来说，通过手动过程设置 OpenVPN 可能是一个挑战。本指南展示了在 Ubuntu 22.04 上安装 OpenVPN 服务器的简单方法。这是一个脚本化的方法，因此任何具有 Linux 基础知识的人都可以遵循它:

在第一步中，在命令行终端中键入以下命令并开始安装:

```
sudo apt update 
```

```
sudo apt install openvpn
```

然后**创建一个静态密钥**用于 VPN 隧道加密:

```
openvpn --genkey --secret static-OpenVPN.key
```

现在设置一个 OpenVPN 服务器来接收传入的连接请求:

```
sudo openvpn --dev tun --ifconfig your_ip_address_1 your_ip_address_2 --cipher AES-256-CBC --secret static-OpenVPN.key &
```

注意，在上面的命令中，不是 **your_ip_address_1** 和 **your_ip_address_2** ，而是键入您想要的 ip 地址。通过执行上述命令，您不需要打开终端来运行此服务。

现在您的系统有了一个新的网络接口，名为 tun0，带有您的 IP 地址。键入以下命令进行确认:

```
ip a show tun0
```

然后检查系统上的 **UDP 端口 1194** 是否打开，以确保 VPN 服务器正常工作:

```
netstat -anu | grep 1194
```

最后输入以下命令来配置 **Ubuntu 的 UFW 防火墙**以允许 UDP 1194 端口上的传入连接:

```
sudo ufw allow from any to any port 1194 proto udp
```

### **如何使用 OpenVPN 客户端**连接到 OpenVPN 服务器

现在按照本节中的步骤使用 OpenVPN 客户端连接到 OpenVPN 服务器。

打开终端，输入以下命令**开始安装 OpenVPN 服务器**:

```
sudo apt install openvpn
```

在下一步中，您的客户端设备需要一个 OpenVPN 服务器来连接到 **static-OpenVPN.key** 加密密钥文件。使用以下命令通过 **scp** 传输文件:

```
scp [[email protected]](/cdn-cgi/l/email-protection):/home/Evelyn/static-OpenVPN.key .
```

现在使用这个命令**创建一个到服务器的 VPN 隧道**，但是将**YOUR-OPENVPN-SERVER-IP-OR-HOST**字符串替换为您所连接的 VPN 服务器的 IP 地址或主机名:

```
sudo openvpn --remote YOUR-OPENVPN-SERVER-IP-OR-HOST --dev tun --ifconfig your_ip_address_1 your_ip_address_2 --cipher AES-256-CBC --secret static-OpenVPN.key &
```

创建 VPN 隧道后，您将看到以下消息:

```
Initialization Sequence Completed
```

最后，ping 远程网络上的主机，以连接到 VPN 服务器:

```
ping -c 1 your_ip_address 
```

## 结论

在本文中，我们学习了如何使用脚本方法在 Ubuntu 22.04 上设置 OpenVPN 服务器，任何具有 Linux 基础知识的人都可以遵循它。这样，您可以在服务器和客户机之间建立安全的 VPN 连接。