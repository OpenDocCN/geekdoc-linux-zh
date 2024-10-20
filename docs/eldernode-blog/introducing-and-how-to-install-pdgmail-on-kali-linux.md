# 介绍如何在 Kali Linux - Eldernode 博客上安装 pdgmail

> 原文：<https://blog.eldernode.com/introducing-and-how-to-install-pdgmail-on-kali-linux/>

![Introducing and How to Install pdgmail on Kali Linux](img/7e27bc998a4f4d2ad53c92b628c8fc90.png)

Pdgmail 取证工具用于分析 gmail 数据的内存处理。这个工具发现它可以做什么，通过一个内存图像，包括联系人，电子邮件，上次访问时间，IP 地址，主要标题和更多。本文的主题是**介绍如何在 Kali Linux** 上逐步安装 pdgmail。因此，我们试图向您介绍这个工具，并学习您如何安装它。你可以访问 [Eldernode](https://eldernode.com/) 提供的包来购买 [Linux VPS](https://eldernode.com/linux-vps/) 服务器。

## **介绍并安装 Kali Linux 上的 pdgmail**

pdgmail 是一个 Python 脚本，用于从内存图像中提取 gmail 工件。这个工具是为用 pdd 提取的图像而设计的，但也适用于任何内存图像。在这篇文章的续篇中，加入我们来学习如何在 [Kali Linux](https://blog.eldernode.com/install-and-configure-kali-linux-on-vps/) 上安装 pdgmail。

### **Kali Linux 上的 pdgmail 介绍**

您可能有兴趣知道，用于分析进程内存转储的取证工具最早出现在 GBHackers 上的 [Security](https://blog.eldernode.com/tag/security/) 。Pdgmail 工具用于分析 gmail 数据的内存处理。这个实用的工具很容易安装。在下一节中，我们将学习如何在 Kali Linux 上安装 pdgmail。

### **如何在 Kali Linux 上安装 pdgmail**

要在 Kali Linux 上安装 pdgmail，只需运行以下命令。通过执行以下命令，将安装 pdgmail 和所有依赖包:

```
sudo apt-get install pdgmail -y
```

### Kali 上 pdgmail 包中包含的工具:

```
[[email protected]](/cdn-cgi/l/email-protection):~# pdgmail -h
```

```
-f, --file [the file to use (stdin if no file given)]
```

```
-b, --bodies
```

```
-h, --help [prints this]
```

```
-v,--verbose [be verbose (prints filename, other junk)]
```

```
-V,--version [prints just the version info and exits.]
```

### 如何在 Kali Linux 上使用 pdgmail 举例:

```
[[email protected]](/cdn-cgi/l/email-protection):~# pdgmail -v -f file.dmp
```

## 结论

在本文中，我们试图向您介绍 pdgmail。我们还教了你如何在 Kali Linux 上安装 pdgmail。也可以按照 [Kali Linux](https://blog.eldernode.com/tag/kali-linux/) 板块的文章中 Kali Linux 工具相关的教程。如果你有任何疑问或问题，你可以在评论或社区长老节点问我们。