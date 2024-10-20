# 如何在 Ubuntu 20.04 - Eldernode 博客上安装 Sqlninja

> 原文：<https://blog.eldernode.com/install-sqlninja-on-ubuntu-20-04/>

![How To Install Sqlninja On Ubuntu 20.04](img/1c3bd0651229dd426de2ec692c081158.png)

Sqlninja 是一个 SQL 注入工具。SQL 注入利用使用 Microsoft SQL Server 作为后端的 web 应用程序上的漏洞。在 GPLv3 的统治下，斯奎尼亚被释放。Sqlninja 使攻击者能够远程访问易受攻击的数据库，即使在一般环境充满敌意的情况下也是如此。

SQL 注入攻击对 web 应用程序有害的数据库，因此为了保护您的信息，您需要保护您的 web 应用程序。由于 Windows 版本的开发和维护非常耗时，因此 Sqlninja 不支持 Windows。Sqlninja 的设计目标是专业渗透测试人员，他们在工作中可以使用类似 Unix 的机器。加入我们这篇文章，了解如何**在 Ubuntu 20.04** 上安装 Sqlninja。不要错过 2021 优惠，在 [Eldernode](https://eldernode.com/) 上选择合适的套餐，购买自己的 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) 。

## **教程在 Ubuntu 20.04 上安装 SQL inja**

Sqlninja 是在 Gentoo 平台上开发的，可以在 Linux、FreeBSD、Mac OS X 和 iOS 等操作系统上运行。Sqlninja 是一个很棒的 SQL 注入工具。还有一些其他的工具，如 Bobcat、ExploitMyUnion 和鸦片酊。Sqlninja 需要一些 Perl 组件。要在 Ubuntu 中安装 Sqlninja，首先需要安装 Perl 模块。

### **在 Ubuntu 20.04 上安装 SQL inja | Ubuntu 18.04**

打开终端并运行以下命令:

```
perl -MCPAN -e "install Net::RawIP"
```

```
perl -MCPAN -e "install Net::Pcap"
```

```
perl -MCPAN -e "install Net::PcapUtils"
```

```
perl -MCPAN -e "install Net::Packet"
```

```
perl -MCPAN -e "install Net::DNS"
```

```
perl -MCPAN -e "install IO::Socket::SSL"
```

然后，您可以[下载](http://sqlninja.sourceforge.net/download.html)Sqlninja 来使用它。此外，您可以使用下面的命令下载并解压缩 Sqlninja 文件夹。打开终端并键入:

```
wget https://sourceforge.net/projects/sqlninja/files/sqlninja/sqlninja-0.2.999-alpha1.tgz tar zxvf sqlninja-0.2.999-alpha1.tgz cd sqlninja-0.2.999-alpha1.tgz
```

要查看在终端中运行 Sqlninja 的所有可能选项，请使用以下命令:

```
root @ kali: ~ # sqlninja Sqlninja rel. 0.2.6-r1 Copyright (C) 2006-2011 icesurfer Usage: / usr / bin / sqlninja -m : Required. Available modes are: t / test - test whether the injection is working f / fingerprint - fingerprint user, xp_cmdshell and more b / bruteforce - bruteforce sa account e / escalation - add user to sysadmin server role x / resurrectxp - try to recreate xp_cmdshell u / upload - upload a .scr file s / dirshell - start a direct shell k / backscan - look for an open outbound port r / revshell - start a reverse shell d / dnstunnel - attempt a dns tunneled shell i / icmpshell - start a reverse ICMP shell c / sqlcmd - issue a 'blind' OS command m / metasploit - wrapper to Metasploit stagers -f : configuration file (default: sqlninja.conf) -p : with password -w : wordlist to use in bruteforce mode (dictionary method only) -g: generate debug script and exit (only valid in upload mode) -v: verbose output -d : activate debug 00 - print each injected command 1 - print each raw HTTP request 2 - print each raw HTTP response all - all of the above ... see sqlninja-howto.html for details
```

## 学习在 Ubuntu 服务器上安装 SQL inja

就是这样！安全应该是你关心的第一重要因素。要成为专家，请阅读我们的[安全教程](https://blog.eldernode.com/tag/security/)。

## 结论

在本文中，您了解了如何在 Ubuntu 20.04 上安装 Sqlninja。下载并安装 Sqlninja 工具，以便在渗透测试中使用，并享受它的特性。如果您有兴趣阅读更多关于 SQL inja 及其优势的内容，请关注我们的文章[介绍以及如何在 Kali Linux 上安装 SQL inja](https://blog.eldernode.com/introducing-and-install-sqlninja-on-kali-linux/)。