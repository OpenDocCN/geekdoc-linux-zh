# 如何在 VPS - Eldernode 博客上查看 Linux 内核版本

> 原文：<https://blog.eldernode.com/check-linux-kernel-version-on-vps/>

![How to check Linux kernel version on VPS](img/a24d32a4eca706a976d1abe08a51887e.png)

了解**如何在 VPS** 上检查 Linux 内核版本。在使用 Linux 时，您需要注意的一件事是了解系统信息。你一定想过，要在 Linux 服务器上安装一些软件，你需要知道你的 Linux 服务器是什么版本？或者现在是什么版本的内核，或者你的服务器安装的版本是 32 位还是 64 位？有几种方法可以检查内核版本，我们将在下面提到。在这篇文章中，我们试图教你**如何以不同的方式检查 CentOS、Debian 和 Ubuntu** 发行版中的内核版本。你可以查看 [Eldernode](https://eldernode.com/) 上提供的软件包，购买 [CentOS VPS](https://eldernode.com/centos-vps/) 和 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) 服务器。

## 教程在 VPS 上检查 Linux 内核版本

### 什么是内核？

内核是操作系统的核心，分配 CPU、内存等系统资源。到其他应用程序。内核为想要为他们的平台开发应用程序的开发人员提供了抽象的概念。这些抽象概念包括复杂任务的简化。

内核不仅仅是包含抽象概念的操作系统的软件部分，它实际上是操作系统最重要的部分之一。硬件驱动通过内核与其他部门通信。因此，内核不需要学习如何谈论构成它的各个硬件。这就是为什么一个内核可以在如此多不同品牌和型号的硬件上单独运行。在本文中，我们将教你如何在 CentOS、Debian 和 Ubuntu 中查看 Linux 内核版本。

有几种方法可以检查内核版本，下面是其中的两种:

使用 **uname** 命令，您可以识别您的机器的内核版本:

```
[[email protected]](/cdn-cgi/l/email-protection):~# uname -r
```

上述命令的输出如下所示:

```
3.13.0-141-generic
```

上述输出的详细信息如下:

**3:** 主内核版本

13: 主要修订

**0:** 次要版本

**141-一般:**次要修复/修订详细信息

第二种方法是读取 **/proc/version/** 文件的内容:

```
[[email protected]](/cdn-cgi/l/email-protection) : ~# cat /proc/version | awk '{print $3}'
```

上述命令的输出如下所示:

```
3.13.0-141-generic
```

## 如何在虚拟服务器上检查 CentOS 内核版本

您可以使用以下命令检查系统的各种参数，包括内核版本:

```
# uname -rv
```

***输出:***

```
3.10.0-693.11.6.el7.x86_64 #1 SMP Thu Jan 4 01:06:37 UTC 2018
```

另外，如果您使用的是 CentOS 7 操作系统，可以使用以下命令查看当前内核版本。另外，如果你使用的是 [VPS](https://eldernode.com/vps/) ，你会看到 **Stab** 命令输出:

```
# uname -r
```

***输出:***

```
2.6.32-042stab125.5
```

如果您是在[专用服务器](https://eldernode.com/dedicated-server/)或 KVM 服务器上使用 CentOS 7 的用户之一，您将在下面针对内核版本的命令输出中看到单词 **el7** 。el7 指**红帽企业**。

```
# uname -r
```

***输出:***

```
3.10.0-693.11.6.el7.x86_64
```

在 CentOS 上检查内核版本的另一种方法是使用 **yum** 命令:

```
yum info kernel -q
```

该命令包含系统的更多详细信息。所以可能需要一段时间来实现。

## 教程检查 VPS 上的 Debian 内核版本

您可以使用下面的命令来检查 VPS 上 Debian 发行版的内核版本:

```
$ uname -srm
```

上述命令的输出如下所示:

```
Linux 4.15.0-54-generic x86_64
```

上述输出的详细信息如下:

**4:** 内核版本。
**15:** 重大改版。
0:小改版。
**54:** 补丁号。
**通用:**配送特定信息。

查看和审查 Debian 发行版内核版本的另一种方法是使用以下命令:

```
$ dpkg -l | grep linux-image
```

上面的命令使用 Aptitude 作为前端来执行操作:

## 如何检查 Ubuntu 内核

您可以使用以下命令来检查 [Ubuntu 发行版](https://blog.eldernode.com/introducing-ubuntu-20/)中的系统信息。该命令显示静态主机名、图标名称、机箱、机器 ID、引导 ID、操作系统、内核和体系结构的系统规范。

```
$ hostnamectl
```

***输出:***

**静态主机名:** linuxize.localdomain
**图标名称:**电脑-笔记本电脑
**机箱:**笔记本电脑
**机器 ID:**af 8 ce 1d 394 b 844 Fe A8 c 19 ea 5 c 6 a 6 BD 08
**开机 ID:**15 BC 3 AE 7 bde 842 f 29 c 8d 925044 f 122 b 9
操作系统:

**如果您只想查看内核版本，可以使用以下命令:**

```
`$ hostnamectl | grep -i kernel`
```

*****输出:*****

```
`Kernel: Linux 4.15.0-54-generic`
```

**查看和审查 [Ubuntu](https://blog.eldernode.com/tag/ubuntu/) 发行版内核版本的另一种方法是使用以下命令:**

```
`$ cat /proc/version`
```

*****输出:*****

```
`Linux version 4.15.0-54-generic ([[email protected]](/cdn-cgi/l/email-protection)) (gcc version 7.4.0 (Ubuntu 7.4.0-1ubuntu1~18.04.1)) #58-Ubuntu SMP Mon Jun 24 10:55:24 UTC 2019`
```

****结论****

**使用 Linux 操作系统时，您应该知道的一件事是了解系统信息。您需要知道的信息之一是内核版本。在本文中，我们试图教你如何在 VPS 上检查不同的 Linux 发行版中的内核版本。**