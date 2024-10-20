# Linux - Eldernode 中的关机关机关机暂停和重启命令是什么

> 原文：<https://blog.eldernode.com/shutdown-poweroff-halt-reboot-commands-linux/>

![What is Shutdown Poweroff Halt and Reboot Commands in Linux](img/880922407451357a08d4a2c8f5daf2f2.png)

一个 Linux 系统管理员需要知道一些 Linux 技巧。在这篇文章中，你将学习到什么是 Linux 中的关机关机暂停和重启命令。让我们来看看**关机**、**断电**、**停止、**和**重启** [Linux](https://www.linux.org/) 命令之间的区别，以明确当您使用可用选项执行它们时，它们实际上做了什么。

### Linux 中什么是关机断电暂停和重启命令

您将回顾一些重要的 [Linux 命令](https://eldernode.com/tag/linux-commands-tutorial/) ，为了有效和可靠的服务器管理，您需要完全理解这些命令。所以我们从关闭或重启机器所需的命令开始。

### 关机命令

要安排系统关机的时间，您可以使用**关机、**暂停、关闭或重启机器。此外，您可以指定一个时间字符串作为第一个参数，或者设置一个在系统关闭之前发送给所有登录用户的 wall 消息。

**请注意** :在使用时间参数的情况下，会创建 **/run/nologin** 文件，以确保在系统关闭前 5 分钟不允许进一步登录。

一些**关机**命令:

```
shutdown  shutdown now  shutdown 13:20    shutdown -p now	        poweroff the machine  shutdown -H now	        halt the machine		  shutdown -r09:35	reboot the machine at 09:35am
```

要取消挂起的关机:

```
shutdown -c 
```

**您可能会感兴趣:**

[如何在 Linux 中禁用关机和重启命令](https://eldernode.com/disable-shutdown-reboot-linux/)

[How to disable shutdown and reboot commands in Linux](https://eldernode.com/disable-shutdown-reboot-linux/)

停止命令

### 当您运行 **halt** 命令时，它会指示硬件停止所有 CPU 功能。同时保持其通电。要使系统进入可以执行低级维护的状态，可以使用以下命令。

**请注意** :你会面临一些完全关闭系统的情况。

一些**暂停**命令:

```
halt		   halt the machine  halt -p	           poweroff the machine  halt --reboot      reboot the machine
```

断电命令

### **断电**命令发送 ACPI 信号，指示系统断电。

一些 **断电** 命令:

```
poweroff   	       poweroff the machine  poweroff --halt        halt the machine  poweroff --reboot      reboot the machine
```

重启命令

### **重启**指示系统重启。

```
reboot            reboot the machine  reboot --halt     halt the machine  reboot -p   	  poweroff the machine
```

**好样的** ！您已经完成了本教程，从现在开始，通过理解这些命令，您将能够在多用户环境中有效、可靠地管理 Linux 服务器。要了解更多，请阅读 [Linux 技巧](https://eldernode.com/tag/linux-tricks/) 文章。

亲爱的用户，我们希望本教程能对你有所帮助，如有任何问题或想查看我们的用户关于本文的对话，请访问 [提问页面](https://eldernode.com/ask) 。也是为了提高自己的见识，准备了这么多有用的教程给 [Eldernode 培训](https://eldernode.com/blog/) 。

Dear user, we wish this tutorial would be helpful for you, to ask any question or review the conversation of our users about this article, please visit [Ask page](https://eldernode.com/ask). Also to improve your knowledge, there are so many useful tutorials ready for [Eldernode training](https://eldernode.com/blog/).