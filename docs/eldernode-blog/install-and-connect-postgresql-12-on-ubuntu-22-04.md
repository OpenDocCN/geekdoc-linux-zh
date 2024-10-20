# 如何在 Ubuntu 22.04 上安装和连接 PostgreSQL 12

> 原文：<https://blog.eldernode.com/install-and-connect-postgresql-12-on-ubuntu-22-04/>

![How-to-Install-and-Connect-PostgreSQL12-on-Ubuntu-22.04-2](img/fe71a9cd20bf0b9eb7bfe29bbff82b4a.png)

除了数据库，还有维护数据库的服务，称为数据库管理或 DBMS。其中一个 DBMSs 正在调用 PostgreSQL。本文将帮助你熟悉 PostgreSQL，并学习如何在 Ubuntu 22.04 上安装和连接 PostgreSQL 12。可以在 [Eldernode](https://eldernode.com/) 网站上查看购买 [**Ubuntu VPS**](https://eldernode.com/ubuntu-vps/) 服务器包。

## **教程在 Ubuntu 22.04 上安装连接 PostgreSQL 14**

### **PostgreSQL 简介**

PostgreSQL 是一个基于 SQL 的免费开源数据库管理系统。该数据库支持 SQL 和 JSON 进行关系和非关系查询，以便使用 SQL 进行开发。PostgreSQL 可以包括各种高级信息和性能优化特性。

### **PostgreSQL 12**中提供的特性

–>改进分布式数据的高性能和工作负载。

–>查询并行化和逻辑迭代。

–>上面写的工作量。

–>同步方面的进步。

–>安全性增强。

在本文的剩余部分，我们将一步步教你如何在 [Ubuntu](https://blog.eldernode.com/tag/ubuntu/) 22.04 上安装和连接 PostgreSQL 12。

## **如何在 Ubuntu 22.04 上安装 PostgreSQL**

首先**更新和升级**您的系统:

```
sudo apt update  sudo apt -y install vim bash-completion wget  sudo apt -y upgrade
```

然后**重启**你的系统:

```
sudo reboot
```

现在**使用以下命令导入 GPG 密钥**:

```
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```

您必须**将新版本的 PostgreSQL** 添加到操作系统的默认包存储库中:

```
apt policy postgresql
```

输入用于签署包裹的 **GPG 密钥**:

```
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```

使用以下命令**在 Ubuntu 22.04 上添加 PostgreSQL 存储库**:

```
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee /etc/apt/sources.list.d/pgdg.list
```

运行以下命令，向系统通知新添加的存储库:

```
sudo apt update
```

现在**用下面的命令在 Ubuntu 22.04 上安装 PostgreSQL 12** :

```
sudo apt install postgresql-12 postgresql-client-12
```

您可以使用以下命令检查 PostgreSQL 状态:

```
systemctl status postgresql.service
```

使用以下命令设置每次系统重新启动后启动:

```
systemctl status [[email protected]](/cdn-cgi/l/email-protection)
```

### **如何测试 PostgreSQL 连接**

注意，postgres 用户是在安装过程中自动创建的。首先，使用以下命令赋予您的帐户 sudo 权限:

```
sudo su - postgres 
```

现在您需要将用户密码重置为强密码:

```
psql -c "alter user postgres with password 'StrongAdminPassw0rd'"
```

然后通过输入以下命令启动 PostgreSQL:

```
psql 
```

要获取连接详细信息，请使用以下命令:

```
\conninfo
```

现在，您应该使用以下命令创建一个测试数据库和用户:

```
CREATE DATABASE mytestdb;
```

```
CREATE USER mytestuser WITH ENCRYPTED PASSWORD '[[email protected]](/cdn-cgi/l/email-protection)';
```

若要将数据库 mytestdb 上的所有权限授予 mytestuser，请运行以下命令:

```
GRANT ALL PRIVILEGES ON DATABASE mytestdb to mytestuser;
```

您可以通过运行以下命令列出创建的数据库:

```
\l
```

然后通过运行以下命令连接到数据库:

```
\c mytestdb
```

您可以使用 createuser 来创建用户:

```
createuser myuser --password
```

要使用 createdb 创建数据库，请使用以下命令:

```
createdb mydb -O myuser
```

最后，运行以下命令:

```
psql -l
```

## `结论`

`PostgreSQL 是维护数据库的服务之一。在本文中，您熟悉了 PostgreSQL 14 中的新特性，还学习了如何在 Ubuntu 22.04 上安装和连接 PostgreSQL 12，我们还研究了 PostgreSQL 14 的配置方法。`