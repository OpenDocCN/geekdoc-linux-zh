# 如何在 Ubuntu 20.04 - Eldernode 博客上安装 THC Hydra

> 原文：<https://blog.eldernode.com/install-thc-hydra-on-ubuntu/>

![How To Install THC Hydra On Ubuntu 20.04](img/7b5f5b581ab694a7648db337b0436ab3.png)

**Hydra** ，也称为 THC Hydra，是一个非常强大和专业的破解工具，使用这个工具来破解不同的帐户。该工具支持不同的攻击协议。九头蛇有一个图形化的环境和国际支持。它还支持 HTTP、socks 代理和 51 种不同的协议。更重要的是，它支持大多数操作系统，如所有 Unix 平台、macOS、Windows、基于 Linux、macOS 或 QNX 的移动系统。Hydra 有命令和图形两种格式。默认存在于 Kali Linux 等黑客操作系统中。在这篇文章中，你将学习**如何在 Ubuntu 20.04** 上安装 THC Hydra。访问 [Eldernode](https://eldernode.com/) 的可用软件包，找到一个符合您需求的软件包，并购买您自己的 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) 。

为了让本教程更好地发挥作用，请考虑以下**先决条件**:

拥有 sudo 权限的非 root 用户。

要进行设置，请遵循我们在 Ubuntu 20.04 上的[初始服务器设置。](https://blog.eldernode.com/initial-server-setup-on-ubuntu-20/)

## **教程在 Ubuntu 20.04 上安装 THC Hydra**

Hydra 是在 AGPL 3.0 许可下用 C 语言编写的，支持多种攻击协议。作为一个非常快速和灵活的工具，可以很容易地添加新的模块。研究人员和安全顾问使用该工具来展示获得对系统的远程授权访问是多么容易。Hydra 和许多其他类似的笔测试工具和程序被称为 Bruce Force，这是一种常见的方法，也是他们使用的方法。

### **支持的协议**

Hydra 支持下面列出的一些协议:

1-启用思科

2- FTP

3- HTTP(S)-FORM-GET-POST

4- HTTP 代理

5- IMAP

6 毫秒 SQL

7- MySQL

8- NNTP

9- IRC

10 件套 NFS

11- POP3

12- PostgresSQL

13- RDP

14- SMB

15- SMTP

16- SNMP

17-足球 5

18-嘘

19-团队发言

20-远程登录

21- VMware 授权

22- VNC

诸如此类。

### **在 Ubuntu 20.04 上快速安装九头蛇| Ubuntu 18.04**

总是建议选择一个[强密码](https://blog.eldernode.com/how-to-create-strong-password/)。因为猜测和破解密码已经变得很容易，暴力破解成了流行的主要攻击方式。破解密码的强力工具是九头蛇。在 Ubuntu 上安装 Hydra 非常简单快捷。Hydra 通常预装在 [Kali Linux](https://blog.eldernode.com/tag/kali-linux/) 系统中，但是现在你正在使用另一个发行版，按照下面的步骤继续安装。

打开终端并运行以下命令，在 Ubuntu 上安装 hydra 软件包:

```
sudo apt-get update
```

```
sudo apt-get install hydra
```

**注意**:当你使用 *-y* 标志时，你的意思是“是”并提供一个没有问题的静默安装。

要安装一些依赖库，请键入:

```
apt install libssl-dev libssh-dev libidn11-dev libpcre3-dev libgtk2.0-dev libmysqlclient-dev libpq-dev libsvn-dev firebird-dev libncp-dev
```

如果您在存储库中没有找到这些库，请手动下载并安装它们。

### **如何从 Ubuntu Linux 中移除九头蛇**

如果您不小心安装了它，或者出于某种原因希望卸载 hydra，请运行下面的命令将其删除。

```
sudo apt-get purge hydra-gtk
```

```
sudo apt-get autoremove
```

```
sudo apt-get autoclean
```

## 结论

在本文中，你已经学习了如何在 Ubuntu 20.04 上安装 THC Hydra。Hydra 使用不同的方法来执行暴力攻击，并猜测正确的用户名和密码组合。[渗透测试人员](https://blog.eldernode.com/configure-beef-on-ubuntu-20-04/)使用 Hydra 和一组程序来生成单词表。