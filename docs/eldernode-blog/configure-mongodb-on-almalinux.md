# 在 Almalinux 上配置 MongoDB 的两种方法

> 原文：<https://blog.eldernode.com/configure-mongodb-on-almalinux/>

![2 ways to Configure MongoDB on Almalinux](img/4e45462ac4da93751d4ec68ff4046229.png)

随着全球数据量的增长，企业正在寻找新的方法来管理数据和信息。今天，关系数据库(SQL)和非关系数据库(NoSQL)被用来管理数据。非关系数据库之一是 MongoDB，它被认为是 NoSQL 小组中最流行的数据库。本文将解释在 Almalinux 上配置 MongoDB 的两种方法。 [Eldernode](https://eldernode.com/) 网站提供经济实惠的 [Linux VPS](https://eldernode.com/linux-vps/) 服务器，如果你想购买的话可以是最好的选择。

## **如何在 Almalinux 服务器上安装 MongoDB**

MongoDB 是一个免费的、开源的、跨平台的数据库管理程序，属于 NoSQL 数据库的范畴。事实上，这个工具可以管理面向文档的信息，并存储或检索信息。它由 MongoDB Inc .开发，用 C++、C 和 JavaScript 语言编写。该数据库使用 JSON 文档，并在 GNU Affero 通用公共许可证和 Apache 许可证的联合许可下发布。

### **1-在 AlmaLinux 上安装 MongoDB**

第一步是添加 MongoDB 存储库。如您所知， [Almalinux](https://blog.eldernode.com/tag/almalinux/) 不包含 MongoDB 包，您应该创建如下所示的 MongoDB 存储库:

```
sudo vim /etc/yum.repos.d/mongodb-org.repo
```

并添加以下配置，以便能够安装最新版本:

```
[mongodb-org-6.0]  name=MongoDB Repository  baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/6.0/x86_64/  gpgcheck=1  enabled=1  gpgkey=https://www.mongodb.org/static/pgp/server-6.0.asc
```

现在使用下面的命令更新系统存储库，将 MongoDB 存储库与系统同步:

```
sudo dnf update
```

是时候在您的 AlmaLinux 上安装 MongoDB 了。为此，只需运行以下命令:

```
sudo dnf install mongodb-org
```

按 **Y** 和**回车**导入 MongoDB GPG 密钥。

您可以使用以下命令验证 MongoDB 版本的安装:

```
mongod --version
```

现在检查 **MongoDB 状态**，如下所示:

```
sudo systemctl status mongod
```

您需要使用以下命令启动 MongoDB 守护进程:

```
sudo systemctl start mongod
```

并使用下面的命令**启用**:

```
sudo systemctl enable mongod
```

再次验证 MongoDB 状态，以确认它已经运行:

```
sudo systemctl status mongod
```

您可以使用下面的命令登录到 Mongo shell:

```
mongo
```

### **2-用 MongoDB** 管理数据库

以下命令将显示当前的数据库。 [MongoDB](https://blog.eldernode.com/5-things-that-make-mongodb-better-than-mysql/) 默认提供一个测试数据库，称为 test:

```
> db
```

您可以使用以下命令创建数据库:

```
> use eldernode-db
```

MongoDB 将数据存储在称为文档的记录中，并采用类似 JSON 的格式。此外，条目以键值对的形式存在。

在这一步中，创建一个文档并添加一些数据，如下所示。记得在 MongoDB 提示符下添加这个，并按下 **Enter** :

```
db.students.insertOne(     { "First Name" : "Marilyn",       "Last_Name" : "Bisson",       "City" : "Piscataway",       "Id No." : 123456,       "Age" : 24     }  )
```

您可以使用下面的命令查看数据库中的文档:

```
> show collections
```

以下命令可以显示文档中存储的数据:

```
> db.students.find()
```

如果要删除文档，请输入以下命令:

```
db.students.drop()
```

就是这样！

## 结论

MongoDB 是可以用于数据库管理的 NoSQL 数据库之一。在本文中，我们向您介绍了 MongoDB，并教您如何以两种方式在 AlmaLinux 上配置 MongoDB。我希望这篇教程对你有用，并帮助你在 AlmaLinux 上安装 MongoDB。如果你面临任何问题或有任何疑问，可以在评论区联系我们。