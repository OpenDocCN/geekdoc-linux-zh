# 在 Kali Linux 上安装 Bluesnarfer 的 4 个步骤

> 原文：<https://blog.eldernode.com/install-bluesnarfer-on-kali-linux/>

![4 Steps To Install Bluesnarfer on Kali Linux](img/210edc0082bd929b4abfe068bba0ab69.png)

您可能在各种设备上启用了蓝牙，包括您的电话、笔记本电脑和 PC。本文将教你如何在 Kali Linux 上安装 Bluesnarfer。您的蓝牙连接对黑客来说并不安全。网络犯罪分子可以将您的蓝牙设备作为攻击目标。Bluesnarf 攻击是最常见的蓝牙攻击之一。你可以查看 [Eldernode](https://eldernode.com/) 网站上提供的软件包，购买自己的 [Linux VPS](https://eldernode.com/linux-vps/) 服务器。

## **如何在 Kali Linux 上安装 blues narfer**

Bluesnarfer 是一个蓝牙搜索工具，用于未经授权访问使用蓝牙通信的无线设备的信息。实际上，该工具用于扫描远程设备地址以获取设备信息。在这种攻击中，被称为 Bluesnarfer 的攻击者通过蓝牙向您的设备发送对设备有危险的信息。任何蓝牙连接打开并设定为“可被发现”的设备都可能受到攻击。通过关闭此功能，您可以保护自己免受可能的 Bluesnarf 攻击。

在这个来自 [Kali Linux 培训](https://blog.eldernode.com/tag/kali-linux/)系列的教程中，你将学习如何在 Kali Linux 上安装 Bluesnarfer。

### **第一步:更新系统包**

运行以下命令更新 Kali Linux 系统软件包:

```
sudo apt update
```

### **第二步:在 Kali Linux** 上安装 blues narfer

在这一节中，我们将用 4 个步骤教你在 Kali Linux 上安装 Bluesnarfer 的过程。

是时候在你的 [Kali Linux 服务器](https://blog.eldernode.com/introduction-kali-linux-server-and-its-applications/)上安装 Bluesnarfer 了。为此，只需输入以下命令:

```
sudo apt install Bluesnarfer
```

### **第三步:在 Kali Linux 上运行 blues narfer**

您可以使用下面的命令运行 Bluesnarfer 实用程序:

```
./bluesnarfer
```

Bluesnarfer 的帮助命令如下:

```
bluesnarfer --help
```

### **第四步:在 Kali Linux 上使用 blues narfer**

下面是 Kali Linux 上 Bluesnarfer 实用程序的一般用法命令:

```
bluesnarfer -b MAC-address [options]
```

***注意:*** 记住在上面的命令中替换你的远程设备地址而不是 MAC-address。

例如，如果使用-i 选项，可以列出设备信息，如下所示:

```
bluesnarfer -b 20:C9:D0:43:4B:D8 -i
```

就是这样！

## 结论

Bluesnarfer utility 是通过蓝牙连接从无线设备未经授权访问信息。它可以从目标设备窃取信息和文件。在本文中，我们用 4 个步骤教你如何在 Kali Linux 上安装 Bluesnarfer。我希望这篇教程对你有用，并帮助你了解什么是 Bluesnarfer 并在你的 Kali Linux 服务器上安装它。如果您有任何问题或建议，可以在评论区联系我们。