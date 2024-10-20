# 如何找到 MySQL PHP 和 Apache 配置文件- Eldernode

> 原文：<https://blog.eldernode.com/find-mysql-php-apache-configuration-files/>

![How to Find MySQL PHP and Apache Configuration Files](img/6211095963f7ee29ce31ce063c78ba31.png)

一个 Linux 系统管理员需要知道一些 Linux 技巧。在本文中，您将学习如何找到 MySQL PHP 和 Apache 配置文件。

为了达到本教程的目的，我们将学习一些用于定位默认配置文件的命令，这些文件用于 **MySQL** 数据库服务器 **(my.conf)** 、PHP 编程语言 **(php.ini)** 和 Apache HTTP 服务器 **(http.conf)** ，它们与 Linux 一起构成了 [灯(Linux Apache MySQL/MariaDB PhD b](https://eldernode.com/tag/lamp-stack/)

什么是配置文件？配置文件包含与系统相关的或应用程序的设置，这些设置使开发人员和管理员能够控制系统或应用程序的操作。

## 如何找到 MySQL PHP 和 Apache 配置文件

加入我们来学习配置文件的位置或者掌握找到它们的方法，这对于一个 Linux 系统管理员来说是一个无价的技能。另外，我们来说明一下， **/etc** 目录或其子目录中存储了 [Linux 目录结构](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard) 中的系统相关或应用配置文件。

尽管这是配置文件的主要位置。少数开发人员选择将其他配置文件存储在自定义目录中。

[购买 Linux 虚拟私有服务器](https://eldernode.com/linux-vps/)

### 如何找到 MySQL (my.conf)配置文件

您将使用以下命令来显示 **mysql** 或 **mysqladmin** 帮助页面。其中有一节讨论了从中读取默认选项的文件(配置文件)。

注意: **grep** 选项 **-A** 显示匹配行后尾随上下文的 NUM 行。

```
mysql --help | grep -A1 'Default options'  OR  mysqladmin --help | grep -A1 'Default options
```

**同样，阅读**

[教程在 Ubuntu 20.04 Linux 上安装最新的 phpMyAdmin](https://eldernode.com/install-the-latest-phpmyadmin-on-ubuntu-20/)

如何找到 PHP (php.ini)配置文件

### **php** 可以从终端使用 PHP 命令行实用程序进行控制，配合使用 **-i** 开关来显示 PHP 信息。

```
php -i | grep "Loaded Configuration File" 
```

找到 Apache http.conf/apache2.conf 配置文件

### 您可以使用下面的 **apache2ctl** 控制界面来管理 apache2，该界面带有**–****V**标志，显示 apache2 的版本和构建参数。或者直接调用 **apache2** ，这在大多数情况下并不推荐。

```
--------- On CentOS/RHEL/Fedora ---------  apachectl -V | grep SERVER_CONFIG_FILE    --------- On Debian/Ubuntu/Linux Mint ---------  apache2ctl -V | grep SERVER_CONFIG_FILE
```

**好样的** ！通过完成本教程，您学习了如何找到 MySQL PHP 和 Apache 配置文件。

亲爱的用户，我们希望本教程能对你有所帮助，如有任何问题或想查看我们的用户关于本文的对话，请访问 [提问页面](https://eldernode.com/ask) 。也为了提高你的知识，有这么多有用的教程为[老年人节点培训](https://eldernode.com/blog/)准备。

Dear user, we wish this tutorial would be helpful for you, to ask any question or review the conversation of our users about this article, please visit [Ask page](https://eldernode.com/ask). Also to improve your knowledge, there are so many useful tutorials ready for [Eldernode training](https://eldernode.com/blog/).