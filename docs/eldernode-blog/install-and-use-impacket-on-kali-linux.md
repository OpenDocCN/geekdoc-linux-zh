# 如何在 Kali Linux - Eldernode 博客上安装和使用 impacket

> 原文：<https://blog.eldernode.com/install-and-use-impacket-on-kali-linux/>

![How to Install and Use impacket on Kali Linux](img/6784dca7502ac8f858ef5848219852ba.png)

许多刚刚起步的程序员选择 Python 作为他们的第一编程语言。因为 Python 并不局限于特定类型软件的开发，而是应用于各个工作领域。Python 支持使程序模块化和代码可重用的库和模块。Impacket 是最好的用于黑客攻击的 Python 库之一。本文将教你如何在 Kali Linux 上安装和使用 impacket。如果你想购买一台 **[Linux VPS](https://eldernode.com/linux-vps/)** 服务器，你可以查看 [Eldernode](https://eldernode.com/) 网站上提供的软件包。

## **教程在 Kali Linux 上安装和使用 impacket**

### **什么是 impacket？**

Impacket 是 Python 类的集合，提供对网络数据包的访问。事实上，impacket 包含了一组用于处理网络协议的 Python 脚本，并专注于底层程序对网络数据包的访问。这个库允许 Python 开发者以简单的方式设计和解码网络数据包。它使得开发人员可以很容易地在一个框架下工作，并实现一些自定义协议。Python 开发者可以在 impacket 的帮助下轻松创建和解码网络数据包。此外，impacket 支持 IP、UDP 和 TCP 等低层协议以及 NMB 和 SMB 等高层协议。

在这篇来自 [Kali Linux 培训](https://blog.eldernode.com/tag/kali-linux/)系列的文章的续篇中，我们将教你如何在 Kali Linux 上安装和使用 impacket。

## **如何在 Kali Linux 上安装 impacket**

在这一步，我们将教你如何在 Kali Linux 上安装 impacket。只需按照以下步骤，并输入所需的代码。

首先，您应该使用以下命令来更新您的系统:

```
sudo apt-get update
```

在安装 impacket 之前，您需要使用以下命令**安装 Python 3** :

```
sudo apt install -y python3-pip
```

接下来，**使用以下命令下载 impacket** :

```
sudo git clone https://github.com/SecureAuthCorp/impacket.git /opt/impacket
```

现在，您可以通过运行以下命令来安装 impacket :

```
sudo apt install python3-impacket
```

最后运行以下命令:

```
sudo python3 ./setup.py install
```

在 Kali Linux 上成功安装 impacket。在下一步中，您将学习如何在 Kali Linux 上使用 impacket。

### **如何在 Kali Linux 上使用 impacket**

现在我们将解释 impacket 在 Kali Linux 上的用法。

您可以运行以下命令来执行各种技术，以便在不执行任何代理的情况下从远程计算机转储机密:

```
impacket-secretsdump -h
```

然后输入以下代码，下载目标系统的用户列表:

```
impacket-samrdump -h
```

就是这样！在 Kali Linux 上成功安装 impacket。

## 常见问题解答

[sp _ easy agreement]

## 结论

Impacket 是用 Python 编写的，用于构造网络协议，包含几个用于远程服务执行的工具。正如您在上面看到的，impacket 设置过程非常简单。在本文中，我们教您如何在 Kali Linux 上安装和使用 impacket。我希望你喜欢这个教程，它对你有用。如有疑问，可在评论中联系我们。