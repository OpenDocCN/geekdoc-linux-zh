# 教程在 Fedora 36 上安装和连接 MySQL 8

> 原文：<https://blog.eldernode.com/install-and-connect-mysql-8-on-fedora/>

![Tutorial-Install-and-Connect-MySQL-8-on-Fedora-36](img/b2a97ed80f723e38c1e90356c84326ad.png)

数据库被认为是信息技术世界中的基本概念之一，并被用于诸如 web 和应用程序开发的各种领域。存储在数据库中的信息通过数据库管理系统(DBMS)进行管理。最流行的数据库管理系统之一是 MySQL。在本文中，你将学习如何在 Fedora 36 上安装和连接 MySQL 8。您可以访问 [Eldernode](https://eldernode.com/) 网站，选择您想要的软件包来购买 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务器。

## **什么是 MySQL？**

MySQL 是最流行的开源数据库管理系统之一，于 1995 年首次推出。MySQL 可以运行在 Linux、Windows、Unix 等各种操作系统上，定义和管理你的元数据。您可以将它安装在本地系统或服务器上。在 [Fedora 教程](https://blog.eldernode.com/tag/fedora/)系列的这篇文章的续篇中，我们打算一步一步的教你如何在 Fedora 36 上安装 MySQL 8。

## **如何在 Fedora 36 上安装 MySQL 8**

在开始安装之前，运行以下命令**更新您的存储库**:

```
sudo dnf update
```

要**安装 MySQL** ，运行以下命令:

```
sudo dnf install community-mysql-server -y
```

安装后，**使用以下命令检查 MySQL** 的状态:

```
systemctl status mysqld
```

然后**使用下面的命令启动 MySQL** 服务:

```
sudo systemctl start mysqld
```

现在运行以下命令来检查 MySQL 安装是否成功:

```
systemctl status mysqld
```

要在每次启动时设置 MySQL，请使用下面的命令:

```
sudo systemctl enable mysqld
```

为了保证 MySQL 的安全，使用一个安全的脚本。为此，请使用以下命令:

```
sudo mysql_secure_installation
```

如果要求您输入密码，请运行以下命令来检索临时密码:

```
sudo grep 'temporary password' /var/log/mysqld.log
```

### **如何在 Fedora 36 上连接 MySQL 8**

运行以下命令来**连接 MySQL** :

```
sudo mysql -u root -p
```

连接到 MySQL 后，**使用以下命令浏览数据库**:

```
mysql> SHOW DATABASES;
```

**注意**:您可以使用下面的命令在 Fedora 中更新 MySQL:

```
sudo dnf update mysql-server
```

## 结论

这就是你熟悉 MySQL 的方法，MySQL 是最流行的开源数据库管理系统之一。我们还检查了该软件可以在多种操作系统上运行，如 Linux、Windows 等。在本文中，您学习了如何在 Fedora 36 上安装和连接 MySQL 8。