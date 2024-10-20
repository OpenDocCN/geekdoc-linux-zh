# 如何在 Ubuntu 20.04 - Eldernode 博客上安装 Rkhunter

> 原文：<https://blog.eldernode.com/install-rkhunter-on-ubuntu/>

![How to Install Rkhunter on Ubuntu 20.04](img/81acb102e8acc015658a1972aeb6b89e.png)

Rkhunter 是一个用于 Linux 系统的基于 Unix/Linux 的开源扫描工具。这个在 GPL 下发布的工具可以扫描你系统上的后门、rootkits 和本地漏洞。Rkhunter 还扫描隐藏文件、二进制文件上的错误权限设置、内核中的可疑字符串。在本文中，我们将教你如何在 Ubuntu 20.04 上安装 Rkhunter。需要注意的是，如果你想购买一台 **[Ubuntu VPS](https://eldernode.com/ubuntu-vps/)** 服务器，你可以访问 [Eldernode](https://eldernode.com/) 中提供的软件包。

## **教程逐步在 Ubuntu 20.04 上安装 rk hunter**

### **在 Ubuntu 20.04 上安装 rk hunter | Ubuntu 18.04**

在本节中，我们将讨论如何在 [Ubuntu](https://blog.eldernode.com/tag/ubuntu/) 20.04 上安装 Rkhunter。由于 Rkhunter 包在标准的 Ubuntu 存储库中可用，您可以使用以下命令安装它们:

```
apt install rkhunter -y
```

## **如何在 Ubuntu 20.04 上配置 rk hunter**

在您的系统上成功安装 Rkhunter 后，您现在必须配置 Rkhunter，以便能够使用它来扫描您的系统。为此，您需要用您喜欢的编辑器打开 **/etc/Rkhunter.conf** 配置文件。然后进行如下更改:

```
vim /etc/rkhunter.conf
```

现在，您应该在配置文件中将 **UPDATE_MIRRORS** 的值设置为 **1** 。这样做将导致在使用**–更新**选项检查 Rkhunter 更新日期文件时，镜像文件也被检查更新。

```
UPDATE_MIRRORS=1
```

在下一步中，您需要将值 **MIRRORS_MODE** 设置为 **0** 。这个选项告诉 Rkhunter 在选择**-更新**或**-版本**命令行选项时使用哪个镜像。请注意，MIRRORS_MODE 有三个值:

0–使用任何镜像

1–仅使用本地镜像

2–仅使用远程镜像

```
MIRRORS_MODE=0
```

现在，您应该将 **WEB_CMD** 值设置为 null，“”。请注意，该选项必须设置为 Rkhunter 从互联网下载文件时使用的命令。

```
WEB_CMD=""
```

### **如何使用 Cron** 启用定期扫描和更新

你要知道的一点是，Rkhunter 脚本安装在 Cron.d 日常目录下，用于定期扫描和更新。所以脚本每天都由 Cron 执行。因此，您需要编辑**/etc/default/rk hunter . conf**文件，并应用以下更改。您可以通过将 **CRON_DAILY_RUN** 设置为“ **true** ”来启用 Rkhunter 扫描检查，以便每天运行:

```
CRON_DAILY_RUN="true"
```

现在，您还可以再次将 **CRON_DB_UPDATE** 设置为 **true** 来启用对 Rkhunter 数据库的每周更新:

```
CRON_DB_UPDATE="true"
```

如果您想要启用自动数据库更新，也可以将 **APT_AUTOGEN** 的值设置为 **true** :

```
APT_AUTOGEN="true"
```

***注意:*** 完成上述所有更改后，**保存**配置文件并退出。

您可以运行下面的命令来检查任何无法识别的配置选项。应该注意的是，如果发现任何配置问题，就会显示出来，并且返回代码会设置为 1。

```
rkhunter -C
```

或者

```
rkhunter --config-check
```

### **更新 Rkhunter 文本数据文件**

完成前面的步骤后，现在可以运行以下命令来更新 Rkhunter 文本数据文件。应该注意的是，这些文件是 Rkhunter 用来检测系统上的可疑活动的。所以它们必须是最新的:

```
rkhunter --update
```

***注意:*** 由于从[安全](https://blog.eldernode.com/tag/security/)风险的角度来看，运行带有**–更新**的 Rkhunter 可能不是一个好主意，您应该让您的软件包管理员负责更新它。

您也可以通过运行以下命令来获取 Rkhunter 版本:

```
rkhunter --versioncheck
```

## **如何使用 Rkhunter 并执行系统检查**

完成上述所有步骤以正确配置 Rkhunter 后，您现在应该运行以下命令对您的系统执行扫描测试:

```
rkhunter --check
```

有趣的是，为了避免每次检查时都按 ENTER 键，您可以使用以下命令跳过**–sk**或**–skip-keypress**选项:

```
rkhunter --check --sk
```

您也可以使用**–rwo**或**–仅报告警告**选项来仅显示警告信息:

```
rkhunter --check --rwo
```

你要知道 **Rkhunter 登录文件**如下:

```
/var/log/rkhunter.log
```

## 结论

Rkhunter 是一个基于 Unix 的 shell 脚本，可以扫描本地系统中的 rootkits、后门和可能的本地漏洞。在本文中，我们首先尝试教你如何在 Ubuntu 20.04 上安装 Rkhunter。然后我们看了如何配置和使用 Rkhunter。应该注意的是，Rkhunter 还可以控制本地系统命令、启动文件、网络接口的任何更改，以及监听应用程序。