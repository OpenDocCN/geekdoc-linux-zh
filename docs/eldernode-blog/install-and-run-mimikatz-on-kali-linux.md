# 教程在 Kali Linux - Eldernode 上安装和运行 Mimikatz

> 原文：<https://blog.eldernode.com/install-and-run-mimikatz-on-kali-linux/>

![Tutorial Install and Run Mimikatz on Kali Linux](img/c906f8780db5eb1e190fdf8705788059.png)

在 RedTeam 渗透到受害者的网络后，它应该使用工具来执行其活动，以自动化和促进一些现有的流程。假设 RedTeam 在渗透了一个由 Windows 机器组成的网络之后，想要推广和扩展它的活动。在这种情况下，它可以使用基于 PowerShell 或适合 RedTeams 的 C 语言的实用工具。Mimikatz 就是这些工具之一。本文将教你如何在 Kali Linux 上安装和运行 Mimikatz。如果你打算购买一台 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务器，你可以查看 [Eldernode](https://eldernode.com/) 网站上提供的软件包。

## **如何在 Kali Linux 上安装运行 Mimikatz**

### **什么是 Mimikatz？**

Mimikatz 是一款领先的开源后利用工具，使攻击者能够在网络中轻松地进行后利用横向移动。它是由法国开发者 Benjamin Delpy 于 2007 年开发的，用于收集凭证。该工具从内存、哈希、pin 和 Kerberos 票据中转储密码。Mimikatz 可以执行与渗透测试相关的各种操作。

Mimikatz 最初是作为一个研究项目来更好地了解 Windows 安全，也有一个模块可以从内存中删除扫雷，并告诉你所有地雷的位置。Mimikatz 的作者将其描述为“玩弄 Windows 安全的小工具”。

在这篇来自 [Kali Linux 培训](https://blog.eldernode.com/tag/kali-linux/)系列的文章的续篇中，我们打算教你如何在 Kali Linux 上安装和运行 Mimikatz。

## **在 Kali Linux 上安装 Mimikatz**

在本节中，您将学习如何在 Kali Linux 上安装 Mimikatz。遵循以下步骤，并输入以下命令。

首先，**使用以下命令更新您的系统**包:

```
sudo apt update
```

现在是时候使用下面的命令在 Kali Linux 上安装 Mimikatz 了:

```
sudo apt install mimikatz
```

### **如何在 Kali Linux 上运行 Mimikatz**

在这一步中，您将学习如何在 Kali Linux 上运行 Mimikatz。

您可以使用以下命令运行 Mimikatz:

```
mimikatz
```

Mimikatz 的帮助命令如下:

```
mimikatz -h
```

### **在 Kali Linux 上卸载 Mimikatz**

如果您想从 Kali Linux 中删除 Mimikatz 包，请输入以下命令:

```
sudo apt remove mimikatz
```

要**删除 Mimikatz 配置、数据及其所有依赖项**，只需运行以下命令:

```
sudo apt autoremove --purge mimikatz
```

## 结论

Mimikatz 是一个开源工具，旨在揭露微软认证协议中的一个缺陷。它窃取密码，并用于评估针对这些类型攻击的漏洞。在本文中，我们教您如何在 Kali Linux 上安装和运行 Mimikatz。我希望这篇文章对你有用。如果你面临什么问题，可以在评论里问我们。