# 如何在 Ubuntu 18.04 - Eldernode 中创建 AppArmor 配置文件

> 原文：<https://blog.eldernode.com/create-an-apparmor-profile-in-ubuntu-18-04/>

![How To Create An AppArmor Profile In Ubuntu 18.04](img/2a64d7e7a1c71aa68d3e513bfc981365.png)

**App** 应用**装甲**是一个用于安全目的的模块。作为管理员，您需要能够使用预编程配置文件来限制程序的功能。如果你用过 Ubuntu，你肯定用过它的安全特性，但是你没有被告知，因为它是默认包含的，并且在后台静默运行。让我们看看什么是 AppArmor 以及为什么需要创建它。加入我们这篇文章，学习如何**在 Ubuntu 18.04** 中创建 AppArmor 档案。如果你正在寻找一个完美和安全的 [Linux VPS](https://eldernode.com/linux-vps/) ，购买你首选的 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) 包，继续阅读。

为了让本教程更好地发挥作用，请考虑以下**先决条件**:

拥有 sudo 权限的非 root 用户。

要进行设置，请按照我们的[对 Ubuntu 18.04](https://blog.eldernode.com/initial-setup-ubuntu-18/) 进行初始设置。

## **教程在 Ubuntu 18.04 中创建 AppArmor 配置文件**

AppArmor 配置文件是包含注释的简单文本文件，存储在 ***/etc/apparmor.d*** 目录中。它们提供一些功能，如网络访问、原始套接字访问，以及在匹配路径上读取、写入或执行文件的权限。如果你希望限制这些破坏性的安全漏洞，通过提供传统的 Unix **D** 授权 **A** 访问 **C** 控制模型，该方法可能是有效的。Ubuntu 开发者使用 AppArmor 来限制动作的进程。例如，Fedora 和 Red Hat 中的 SELinux 也像 Ubuntu 中的 AppArmor 一样被默认使用。它们的功能是一样的，提供 **M** 和 **A** 访问 **C** 控制。AppArmor **支持**对文件、Linux 功能、网络、挂载、重新挂载和卸载、pivot_root、ptrace、signal、DBus 和 Unix 域套接字的访问控制。

### 创建一个外观档案

您可以使用以下命令来检查 AppArmor 的**状态**:

```
sudo apparmor_status
```

**首先**，要安装您想要限制的应用程序和一些有用的 AppArmor 实用程序，运行下面的命令:

```
sudo apt install apparmor-easyprof apparmor-notify apparmor-utils certspotter
```

### 如何生成基本档案

建议您使用最简单的启动方法。创建骨架轮廓。然后，您可以为您的目标设置 Apparmor to complain 模式，以使用 *aa-logrof* 工具来评估拒绝。要生成框架策略，您将使用 ***aa-easyprof*** 。

```
aa-easyprof /usr/bin/certspotter
```

```
# vim:syntax=apparmor  # AppArmor policy for certspotter  # ###AUTHOR###  # ###COPYRIGHT###  # ###COMMENT###
```

```
#include <tunables/global>    # No template variables specified    "/usr/bin/certspotter" {  #include <abstractions/base>    # No abstractions specified    # No policy groups specified    # No read paths specified
```

```
# No write paths specified  }
```

将输出写入配置文件。

```
aa-easyprof /usr/bin/certspotter > usr.bin.certspotter  sudo mv usr.bin.certspotter /etc/apparmor.d
```

使用下面的命令将概要文件加载到内核中:

```
sudo apparmor_parser -r /etc/apparmor.d/usr.bin.certspotter
```

我们将为 certspotter 生成一个外貌特征。它还没有任何配置文件，是 Ubuntu 中的一个新工具。certspotter 所做的是监视证书透明性日志，以查看是否为监视列表中列出的域生成了新的日志。作为一名证书管理员，您需要设置一个 corn 作业，以便定期监控新条目。现在，让我们使用这个有用的工具。

运行 certspotter 会导致立即(安全)崩溃。

```
certspotter  certspotter: /home/testuser/.certspotter/watchlist: open /home/testuser/.certspotter/watchlist permission denied
```

没有机会浏览源代码，所以您可以限制它在您的系统上的内容。因为基本配置文件不允许 certspotter 访问它需要的资源，所以您可以查看 AppArmor 拒绝消息来检查这里出了什么问题。

### 什么是外观和投诉模式

如果您安装了 audit，对于非 DBus 政策违规，AppArmor 拒绝将被记录***/var/log/audit/audit . log**。*否则会记录到 ***/var/log/syslog** 。*表观拒绝率可能会在分析时造成一些问题，内核将限制它们。因此，在内核中安装 auditd 或调整速率限制来阻止这种情况发生:

```
sudo sysctl -w kernel.printk_ratelimit=0
```

还有一种方法来查看表面上的否认。您可以使用 aa-notify 工具。你会发现这是一个非常简单的程序，可以通过查询 var/log/syslog 来报告任何新的 AppArmor denials。再有如果你已经安装了 audited，它会查询***/var/log/audit/audit . log***。让我们看一个例子。

```
/usr/bin/aa-notify -s 1 -v
```

它会显示出最近一天所有的否认。如果您使用 ***aa-logprof*** 工具来评估 AppArmor 在投诉模式下生成的日志条目并开发这个配置文件，这可能是一个简单的方法。要了解如果您将 certspotter 的 AppArmor 配置文件设置为投诉模式会发生什么情况，请停留在此处。

```
sudo aa-complain certspotter
```

运行 certspotter 一次:

```
certspotter
```

如下所示，它会立即开始在日志中生成 AppArmor 条目。

```
Dec 24 13:34:24 tutorials audit[18643]: AVC apparmor="ALLOWED" operation="recvmsg" profile="/usr/bin/certspotter" pid=18643 comm="certspotter" laddr=10.0.2.15 lport=46314 faddr=10.0.2.16 fport=443 family="inet" sock_type="stream" protocol=6 requested_mast="receive" denied_mask="receive"
```

因为您尚未创建允许它访问网络的配置文件规则。

### 如何使用 aa-logprof 优化概要文件

您可以通过 aa-logprof 解析 AppArmor 消息并建议策略规则。它将允许 certspotter 在监禁下运行。

```
sudo aa-logprof    Reading log entries from /var/log/syslog. Updating AppArmor profiles in /etc/apparmor.d. Complain-mode changes: Profile: /usr/bin/certspotter Path: /proc/sys/net/core/somaxconn New Mode: r Severity: 6 [1 - /proc/sys/net/core/somaxconn r,] (A)llow / [(D)eny] / (I)gnore / (G)lob / Glob with (E)xtension / (N) ew / Audi(t) / Abo(r)t / (F)inish A
```

```
Profile: /usr/bin/certspotter Path: /etc/nsswitch.conf New Mode: r Severity: unknown
```

让 certspotter 读取这个文件，它指定了打开套接字连接的最大数量。键入 A 以允许它。

```
[1 - #include <abstractions/nameservice>] 2 - /etc/nsswitch.conf r, (A)llow / [(D)eny] / (I)gnore / (G)lob / Glob with (E)xtension / (N) ew / Audi(t) / Abo(r)t / (F)inish A
```

certspotter 从证书透明性日志中检索信息，它使用网络来完成这项工作。您可以随意使用现有的名称服务抽象，它允许常见的访问模式，或者您也可以特别允许这种与网络相关的首次访问。在***/etc/apparmor . d/abstracts/name service***中你可以查看抽象的细节。

```
Profile: /usr/bin/certspotter  Path: /proc/sys/kernel/hostname  New Mode: r  Severity: 6    [1 - /proc/sys/kernel/hostname r,]  (A)llow / [(D)eny] / (I)gnore / (G)lob / Glob with (E)xtension / (N) ew / Audi(t) / Abo(r)t / (F)inish    A
```

让 certspotter 知道系统的主机名是可以的。所以，做这个。

```
Profile: /usr/bin/certspotter  Path: /home/testuser/.certspotter/watchlist  New Mode: r  Severity: 4    [1 - /home/*/.certspotter/watchlist r,]  2 - /home/testuser/.certspotter/watchlist r,  (A)llow / [(D)eny] / (I)gnore / (G)lob / Glob with (E)xtension / (N) ew / Audi(t) / Abo(r)t / (F)inish    A
```

为了确定要监视哪些域，certspotter 会读取监视列表。你需要它为系统的所有用户工作，而不仅仅是为你自己。因此，第一个建议的规则比第二个效果更好。既然你知道 certspotter 用的是 ***。certspotter*** 目录来写它发现的信息、它的锁文件和其他数据，所以这个“r”(读)规则将是不够的。此外，您可以使用 ***@{HOME}*** 可调参数，而不是 globbed 路径。现在，接受它作为一个占位符，然后用一个 **TODO** 来润色它。

*注意* : ***@{HOME}*** 是一个可以在轮廓外定义和操作的变量。

```
Profile: /usr/bin/certspotter  Path: /home/testuser/.certspotter/version  New Mode: r  Severity: 4
```

```
[1 - /home/*/.certspotter/version r,]  2 - /home/testuser/.certspotter/version r,  (A)llow / [(D)eny] / (I)gnore / (G)lob / Glob with (E)xtension / (N) ew / Audi(t) / Abo(r)t / (F)inish
```

正如我们提到的，您可以修改监视列表规则，之后，您需要覆盖所有这些关于 ***HOME/中文件的条目。所以你会忽略这些建议的规则。***

```
Enforce-mode changes:    = Change Local Profiles =  The following local profiles were changed. Would you like to save them?    [1 - /usr/bin/certspotter]  (S)ave Changes / Save Selec(t)ed Profile / [(V)iew Changes] / View Change b/w (C)lean profiles / Abo(r)t  S
```

一旦您保存了概要文件， ***aa-logprof*** 将自动重新加载概要文件。它会立即关闭所有关于 certspotter 使用网络的信息。

### 如何手动编辑个人资料

在这一部分，您应该返回并修改概要文件，以允许 certspotter 从 ***HOME/进行读写。*号**目录。

```
sudo vi /etc/apparmor.d/usr.bin.certspotter
```

这里需要改 ***/home/*/。certspotter/watchlist r*** ，line to ***owner @{HOME}/。certspotter/** rw，*** 。

******** glob 表示 certspotter 现在可以读写当前用户的 ***下的所有文件、目录和所有路径。certspotter* 目录在自己的主目录下。是时候用你喜欢的信息修饰一下 **###COMMENT###** 占位符了。然后，再次重新加载策略。**

```
sudo apparmor_parser -r /etc/apparmor.d/usr.bin.certspotter
```

### 表面上否认规则

为了确保 certspotter 不能从 ***HOME*** 中泄露一些数据，您需要添加一些规则。如果你添加明确的规则，他们可以防止配置文件错误，因为默认情况下，默认配置文件默认拒绝。

```
deny @{HOME}/Documents/ rw,  deny @{HOME}/Private/ rw,  deny @{HOME}/Pictures/ rw,  deny @{HOME}/Videos/ rw,  deny @{HOME}/fake/ rw,  deny @{HOME}/.config/ rw,  deny @{HOME}/.ssh/ rw,  deny @{HOME}/.bashrc rw,
```

虽然这个系统上没有假目录，但策略规则仍然有效，如果有一天它被创建了，AppArmor 将对它执行规则。考虑到当您在指定目录本身时使用尾随的“/”时，AppArmor 能够区分文件和目录。所以，记得重装保单。

```
sudo apparmor_parser -r /etc/apparmor.d/usr.bin.certspotter
```

要检查 certspotter 是否工作正常，请运行以下命令。

```
/usr/bin/certspotter
```

如果您看到它没有新的拒绝，您可以将 AppArmor 从该配置文件的投诉模式中取出，并将其设置为强制执行:

```
sudo aa-enforce certspotter
```

为什么拒绝规则不能被允许规则覆盖？因为你不能拒绝 certspotter 所有的 ***@{HOME}*** ，而只允许它访问 ***@{HOME}/。certspotter*** 。

正如项目建议的那样，您可以为 certspotter 添加一个玉米作业来进行最终测试。

```
crontab -e  33 14 * * * /usr/bin/certspotter
```

但是，建议您定期检查是否存在任何新的拒签。如果您希望练习不太常见的代码路径，您可以运行 certspotter 包测试并获得加分。

结论

在本文中，你已经学习了如何在 Ubuntu 18.04 中创建 AppArmor 配置文件。如果你已经从头到尾遵循了这个指南，你应该已经创建了你的第一个 AppArmor 个人资料。为了确保生成，您可以要求某人检查您的最终配置文件，以确保不授予特权。

In this article, you have learned How To Create An AppArmor Profile In Ubuntu 18.04\. If you have followed from A to Z of this guide, you should have created your first AppArmor profile. To ensure the generating, you can ask someone to check your final profile for not granting the privilege.