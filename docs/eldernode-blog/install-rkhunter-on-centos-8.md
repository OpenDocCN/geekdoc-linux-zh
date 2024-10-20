# 教程在 Centos 8 - Eldernode 博客上安装 Rkhunter

> 原文：<https://blog.eldernode.com/install-rkhunter-on-centos-8/>

![Tutorial Install Rkhunter on Centos 8](img/c95bacc3db311488c6ed5a5edd287df9.png)

Rkhunter 或 RootKit Hunter 是一种工具，用于检测安装在 Linux 操作系统上的安全漏洞和后门。它还通过在操作系统上检查文件来扫描文件。因为任何文件都可能存在安全漏洞，黑客可以利用它渗透到操作系统中。RootKit Hunter 充当反病毒引擎，通过检查可疑文件来保护操作系统。在本文中，我们将向您介绍**教程在 CentOS 8** 上安装 Rkhunter。需要注意的是，你可以访问 [Eldernode](https://eldernode.com/) 中的套装来购买一台 **[CentOS VPS](https://eldernode.com/centos-vps/)** 服务器。

## **如何一步步在 Centos 8 上安装 rk hunter**

Rkhunter 是一个 shell 脚本，它在本地系统上执行各种检查。因此，通过这些检查，可以检测到 rootkits 和已知的恶意软件。Rkhunter 还检查命令是否被修改，系统启动文件是否被修改，或者是否在网络接口上执行了各种检查。包括对听力节目的评论。在本教程的后续部分，我们将向您展示如何在 [CentOS](https://blog.eldernode.com/tag/centos/) 8 上安装 Rkhunter。请加入我们。

### **Rkhunter 功能**

Rkhunter 提供的功能之一是扫描文件的修改属性，例如文件完整性搜索引擎使用的一些标准。这完全取决于确保您有正确的数据库进行扫描。一般来说，这可以通过在安装干净的操作系统后立即安装 Rkhunter 来实现。

Rkhunter 不是一个响应工具。仅计算遇到的威胁。由您来阅读日志文件并检查可疑活动。应该注意的是，Rkhunter 团队包含了每个版本的文档，您也可以在网上找到。

另一个信息来源是 Rkhunter 用户邮件列表存档。如果您在这些信息来源中找不到问题的解决方案，想要提出改进建议或想要讨论违反[安全](https://blog.eldernode.com/tag/security/)的问题，我们邀请您加入 Rkhunter 用户邮件列表。如果你想提交一个补丁，你也可以使用我们的 Sourceforge bug tracker。

该扫描工具需要 root 用户电源来执行手动扫描。或者需要 root 权限来创建 Cron 作业。所以需要 root 权限才能在 **/var/log/** 中查看报表。

### **在 Centos 8 上安装 rk hunter | Centos 7**

在本节中，我们将向您展示如何在 Centos 8 上安装 Rkhunter。为此，只需使用以下命令下载最新的 **epel-release rpm** :

```
http://download-ib01.fedoraproject.org/pub/epel/8/Everything/x86_64/
```

下载文件后，您现在需要通过运行以下命令来安装 epel-release rpm:

```
rpm -Uvh epel-release*rpm
```

最后，在成功执行上述命令后，您现在应该通过运行以下命令来安装 Rkhunter Rpm 包:

```
dnf install rkhunter
```

### **如何在 CentOS 8 上配置 rk hunter**

在本节中，我们将讨论如何配置 Rkhunter。请注意，对于定期审查，审查脚本安装在[cron.daily]目录下。所以它可以由 Cron 每天运行。运行以下命令编辑配置文件:

```
vi /etc/sysconfig/rkhunter
```

若要设置收件人发送报告的地址，请运行以下命令:

```
[[email protected]](/cdn-cgi/l/email-protection)
```

使用以下命令更新数据库:

```
rkhunter --update
```

您可以使用以下命令更新系统功能:

```
rkhunter --propupd
```

您可以使用以下命令来执行检查。注意，在下面的命令中[–sk]表示按回车键。[–rwo]也仅表示警告:

```
rkhunter --check --sk
```

### **如何在 CentOS 上使用 rk hunter**

成功安装和配置 Rkhunter 后，您现在可以通过发出以下命令启动手动扫描:

```
rkhunter -c
```

以上命令在交互模式下执行 Rkhunter。也就是说，当扫描完成时，您必须按下**键，输入**才能继续。所以注意，如果你想使用“**自动搜索**交互模式，在最后添加 **-sk** 选项，就像下面的命令:

```
rkhunter -c -sk
```

## 结论

Rkhunter (Rootkit Hunter)是一款针对 POSIX 兼容系统的安全监控和分析工具。该工具帮助用户找到已知的 rootkits 和恶意软件，并标记公共安全漏洞。在本文中，我们试图教你如何在 Centos 8 上安装 Rkhunter。需要注意的是，如果你愿意，可以参考文章[如何在 Ubuntu 20.04](https://blog.eldernode.com/install-rkhunter-on-ubuntu/) 上安装 Rkhunter。