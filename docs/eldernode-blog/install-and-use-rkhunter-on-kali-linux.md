# 如何在 Kali Linux - Eldernode 博客上安装和使用 Rkhunter

> 原文：<https://blog.eldernode.com/install-and-use-rkhunter-on-kali-linux/>

![How to Install and Use Rkhunter on Kali Linux](img/c87d9898c336d912ebf85a4263232404.png)

rootkit 是一组控制计算机系统的软件。在这种类型的攻击中，系统用户不会注意到 rootkit 的存在，计算机黑客将有能力改变所有计算机设置。Rkhunter 或 rootkit hunter 是一个 Unix 工具，可以扫描任何 rootkit、后门程序和任何其他可能的损害模型。在本文中，我们将教你**如何在 Kali Linux** 上安装和使用 Rkhunter。如果你想购买一台 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务器，你可以查看 [Eldernode](https://eldernode.com/) 网站上提供的软件包。

## **教程在 Kali Linux 上安装使用 rk hunter**

### **rk hunter 是什么？**

Rkhunter 被称为 Rootkit Hunter，是一个基于 Unix 的开源扫描工具，可以扫描许多隐藏文件、内核后门中的可疑字符串以及可能的本地漏洞，如后门、Rootkit、电子邮件注入等。这将通过比较重要文件的 SHA-1 哈希与在线数据库中已知的良好文件来完成。Rootkit Hunter 使用一系列工具建立后门并隐藏踪迹，使攻击者能够随时保持对系统的访问并以 root 用户身份登录。

在这篇来自 [Kali Linux 培训](https://blog.eldernode.com/tag/kali-linux/)系列的文章的续篇中，我们打算教你如何在 Kali Linux 上安装 Rkhunter。

## **在 Kali Linux 上安装 rk hunter**

在这一步中，我们将向您展示如何在 Kali Linux 上安装 Rkhunter。为此，只需遵循以下步骤并运行命令。

在执行任何操作之前，**使用以下命令更新**您的系统包:

```
sudo apt update
```

现在您可以通过运行以下命令来安装 Rkhunter :

```
sudo apt install rkhunter
```

### **如何在 Kali Linux 上使用 rk hunter**

在本节中，您将学习 Kali Linux 上 Rkhunter 的配置。只需运行以下命令并按照说明进行操作。

首先，运行以下命令，查看您的计算机是否感染了 rootkit:

```
rkhunter -check
```

您可以使用以下命令**发现未知错误**:

```
rkhunter -c -scan-knownbad-files
```

**Rkhunter 帮助命令**如下:

```
rkhunter -h
```

### **在 Kali Linux 上卸载 rk hunter**

如果你想**卸载 rkhunter** 只需运行以下命令:

```
apt-get -y autoremove rkhunter
```

您可以使用以下命令**删除 rkhunter 配置及其所有依赖项**:

```
apt-get -y autoremove --purge rkhunter
```

## 常见问题解答

[sp _ easy agreement]

## 结论

Rkhunter 检查隐藏和可见的文件、内核模块、默认目录和错误处理的权限。它是 GPL 下的一个 Linux 安全程序，用于扫描 rootkit 和漏洞。在本文中，我们教您如何在 Kali Linux 上安装和使用 Rkhunter。我希望这篇教程对你有用，并且你喜欢它。如有疑问，可在评论中联系我们。