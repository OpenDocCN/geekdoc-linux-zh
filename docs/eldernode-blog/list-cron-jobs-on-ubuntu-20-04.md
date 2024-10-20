# 如何在 Ubuntu 20.04 LTS - Eldernode 博客上列出 Cron Jobs

> 原文：<https://blog.eldernode.com/list-cron-jobs-on-ubuntu-20-04/>

![How to List Cron Jobs on Ubuntu 20.04 LTS](img/14d6342f8c4e2ba049f4681591169b66.png)

Cron 代表命令运行，是控制面板的一个软件特性。Cron Jobs 是 Linux 系统中的一个特性，它负责按照特定的时间表完全自动地执行特定的操作。这个操作可以是命令或特定程序的执行，也可以是 CGI 脚本的执行。例如，为了向用户发送电子邮件而计划执行的 PHP 文件将在指定的时间自动发送，例如，每天上午 9:00。这在 Linux 操作系统中通过 Cron 作业完成，在 Windows 操作系统中通过调度任务完成。在这篇文章中，我们试图向你学习如何在 Ubuntu 20.04 LTS 上列出 Cron Jobs。你可以使用 [Eldernode](https://eldernode.com/) 提供的软件包购买一台 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) 服务器。

## **教程列举 Ubuntu 20.04 上的 Cron Jobs LTS**

一般来说，使用 Cron 作业，您可以轻松地调度和执行日常和重复性任务，而无需完全自动的干预。在每个 Cron 中执行的命令都在调用 Cron 作业。Cron 作业在站点上的一个非常重要的用途是用于用户服务，每次 Cron 实现时，都会向您的用户或客户发送电子邮件，以提供必要的信息。

例如，关于计费信息、服务阻止通知、服务更新通知和服务信息的电子邮件，所有这些都可以通过 PHP 命令轻松配置和执行。如果 Cron Jobs 不使用，所有这些和许多其他事情将不得不手动完成，而且会花费大量时间。在这篇文章中关注我们来解释如何在 LTS Ubuntu 20.04 上列出 Cron 工作。

### **Linux 中的 Cron 作业**

在 Linux 上运行 Cron 作业有一组规则和命令，可以帮助您更容易地定义 Cron 作业。在 [Linux](https://blog.eldernode.com/tag/linux/) 中，每个 Cron 作业由 5 个部分组成，指定为* * * * *。每颗星星代表运行时间。例如，右边的第一颗星表示分钟，即如果您的 Cron 定义为 *** * * * 2** ，Cron Job 每 2 分钟执行一次您的命令。第二颗星表示时钟，可以设置 0 到 23。

比如*** * * 12 ^ 2**表示每天 12 点 2 分，你正在考虑的极限正在执行。第三颗星表示一个月中的第几天，范围从 1 到 31。第四个星号表示一年中的月份，其值可以在 1 到 12 之间调整。最后一颗星是星期几，可以从 0 调整到 6。在最后一个星号中，你应该注意到 0 是星期天。

### **如何在 Ubuntu 上列出当前用户的 Cron 作业**

在开始之前，应该注意默认的 crontab 命令适用于当前用户。使用以下命令来完成此操作:

```
crontab –l
```

确保所有用户 Cron 作业都在/var/spool/cron/crontabs 目录中。同样，在该路径中，为所有用户帐户创建一个单独的文件，其中包含他们的名称。

### **对方用户的 Cron 作业**

您了解了如何列出当前用户的 Cron 作业。在本节中，我们将学习如何列出另一个用户的 Cron 作业。在这种情况下，拥有 root 或 sudo 权限的用户也可以查看其他用户计划的 Cron 任务。您可以使用以下命令列出属于特定用户的所有作业。请注意，在下面的命令中，您必须输入所需的用户名，而不是用户名。

```
sudo crontab –u username –l
```

### **如何列出系统运行的 Cron 作业**

通过以特权 root 或 sudo 帐户的身份运行以下命令，可以轻松查看常见的系统任务。在这种情况下，root 用户可以访问系统 **crontabs** 并编辑和修改它们。

```
less /etc/crontab
```

在本教程中跟随我们，学习如何以每小时、**每天**、**每周**和**每月**为基础列出 Cron 工作。

### **教程列表钟点工**

要查看每小时调度的 Cron 作业，可以使用下面的命令转到 **/ettc/cron.hourly** 。

```
ls -la /etc/cron.hourly
```

上述命令的输出如下。如您所见，每小时执行没有 Cron 工作计划。请注意，您可以查看一个**。每个目录中的占位符**文件。这些目录是由软件包管理器创建的，以防止意外删除列表。

### **教程列表每日 Cron 作业**

与上一步一样，您可以使用以下命令列出每天执行的所有计划任务:

```
ls -la /etc/cron.daily
```

### **教程列表每周 Cron 乔布斯**

命令的一般结构是相似的。因此，在前面的步骤中，使用以下命令可以看到每周 Cron 作业被安排在 **/etc/cron.weekly** 目录中:

```
ls -la /etc/cron.weekly
```

### **教程列表每月 Cron 岗位**

同样，与前面步骤中提到的命令类似，您可以通过运行以下命令轻松列出每月的 Cron 作业:

```
ls -la /etc/cron.monthly
```

## 结论

如上所述，Cron 是 Linux/Unix 操作系统中最有用的工具之一，通常用于与 sysadmin 相关的活动，例如备份或清空 **/tmp/** 目录等等。Cron 服务在后台运行，不断扫描 **/etc/crontab** 文件和 **/etc/cron/** 目录。它还检查了 **/var/spool/cron/** 目录。在本文中，我们试图了解如何在 Ubuntu 20.04 LTS 上列出 Cron 作业。如果您想在 Cpanel 和 DirectAdmin 中激活和配置 cron 作业，可以参考文章[如何在 Cpanel 主机上启用 Cron 作业](https://blog.eldernode.com/how-to-enable-cron-jobs-on-cpanel-host/)和[在 DirectAdmin 中配置 Cron 作业](https://blog.eldernode.com/configure-cron-job-in-the-directadmin/)。