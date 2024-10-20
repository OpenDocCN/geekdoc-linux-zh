# 如何在 Kali Linux - Eldernode 博客上安装和运行 swaks

> 原文：<https://blog.eldernode.com/install-and-run-swaks-on-kali-linux/>

![How to Install and Run swaks on Kali Linux](img/cf303c2db53f5dcf1e9efedbb8c45b72.png)

在本教程中，我们将介绍一个名为 Swaks 的用于渗透测试和 bug 搜索的积极安全工具，并了解更多相关信息。此外，本文将教你如何在 Kali Linux 上安装和运行 swaks。如果你想购买一台 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务器，你可以查看 [Eldernode](https://eldernode.com/) 上提供的软件包。

## **教程在 Kali Linux 上安装并运行 swaks**

### **什么是 Swaks？**

Swaks 被称为瑞士军刀，是测试 SMTP 设置的命令行工具。它是一个灵活的、可脚本化的、面向事务的工具，由 John Jetmore 用 Perl 编写。当您执行渗透测试或 bug 搜索时，您会在侦察过程中遇到许多 SMTP 服务器。该工具在 GNU GPLv2 下获得许可，并允许您使用各种开关来了解许多漏洞，以探索和测试来确定弱点或利用该协议的升级。

### **万字特征**

–包括 SMTP 插件和扩展，如 TLS、认证、管道、PRDR 和 XCLIENT

–支持协议，包括 SMTP、ESMTP 和 LMTP

–通过 Unix 域套接字和互联网域套接字(IPv4 和 IPv6)将数据传输到衍生进程

–完全脚本化的配置，通过环境变量、配置文件和命令行指定选项

## **在 Kali Linux 上安装 Swaks**

在这一节，我们将教你如何在 [Kali Linux](https://blog.eldernode.com/tag/kali-linux/) 上安装 Swaks。首先，您应该以拥有 sudo 权限的用户身份登录。Swaks 可以借助三种方法安装在 [Kali Linux](https://blog.eldernode.com/install-and-configure-kali-linux-on-vps/) 上，分别是:

a) apt，

b) apt-get 和

c)资质。

### **如何使用 apt** 在 Kali Linux 上安装 swaks

通过输入以下命令更新您的系统软件包:

```
sudo apt update
```

通过运行以下命令安装 Swaks:

```
sudo apt install swaks
```

### **如何使用 apt-get 在 Kali Linux 上安装 swaks**

您可以使用 apt-get 命令安装 Swaks:

```
sudo apt-get update
```

```
sudo apt-get install swaks
```

### **如何使用 aptitude** 在 Kali Linux 上安装 swaks

同样，你可以使用 aptitude 来安装 swaks。Aptitude 默认情况下没有安装在 Kali Linux 上，所以您需要在您的 Kali Linux 上安装 aptitude。为此，请运行以下命令:

```
sudo apt install aptitude
```

然后运行以下命令安装 swaks:

```
sudo aptitude update
```

```
sudo aptitude install swaks
```

就是这样！您成功地在 Kali Linux 上安装并运行了 Swaks。

## 常见问题解答

[sp _ easy agreement]

结论

## Swaks 或瑞士军刀 SMTP 是一个测试 SMTP 设置的命令行工具。它允许您随时停止 SMTP 对话。在本文中，我们教你如何在 Kali Linux 上安装和运行 Swaks。我希望这篇教程对你有用。如有疑问，可在评论中联系我们。

Swaks or Swiss Army Knife SMTP is a command-line tool that test SMTP setups. It allows you to stop the SMTP dialog at any time. In this artile, we taught you how to install and run Swaks on Kali Linux. I hope this tutorial was useful for you. If you have any questions, you can contact us in the Comments.