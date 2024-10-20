# 教程在 Rocky Linux 上设置和使用 MongoDB

> 原文：<https://blog.eldernode.com/setup-and-use-mongodb-on-rocky-linux/>

![Tutorial Setup and Use MongoDB on Rocky Linux](img/91642c6f4cdeef4deb05d27ed94e735b.png)

程序员在 web 和应用程序开发中使用不同的数据库。基于 SQL 或 NoSQL 的数据库根据系统需求和程序员的偏好有不同的应用。在本文中，我们打算教您如何在 Rocky Linux 上设置和使用 MongoDB。如果你想[购买 Linux VPS](https://eldernode.com/linux-vps/) 服务器，你可以访问 [Eldernode](https://eldernode.com/) 网站上提供的套装。

## **在 Rocky Linux 上设置 MongoDB 的 1 种方法**

### **MongoDB 及其特性介绍**

[MongoDB](https://blog.eldernode.com/install-mongodb-on-ubuntu-22-04/) 是最著名的 No SQL 数据库之一，结构灵活，多用于数据量大的项目。这个数据库是一个免费的开源平台，使用面向文档的数据模型，可以在 Windows、Macintosh 和 Linux 上使用。存储在 MongoDB 中的数据值与两个主键和辅键一起使用。

MongoDB 包含一个值数组。这些值以文档的形式存在，包含不同大小的不同类型的数据。这个问题使得 MongoDB 能够存储具有复杂结构的数据，如分层或数组数据。

**MongoDB 的一些特性是:**

–由于其面向文档的数据存储模型，MongoDB 比关系数据库更加灵活和可伸缩，并且它解决了许多业务需求。

–该数据库使用分片来划分数据并更好地管理系统。

–可以使用两个主键和辅键来访问数据，并且每个字段都可以被键入。这使得数据访问和处理时间非常快。

–复制是 MongoDB 的另一个重要特性。d

## **如何在 Rocky Linux 上安装 MongoDB**

在上一节熟悉了 MongoDB 及其特性之后，现在这一节我们将教如何在 Rocky Linux 上安装它。为此，只需按顺序执行以下步骤。在第一步中，您必须通过运行以下命令来更新系统:

```
sudo dnf update -y
```

然后，您需要使用以下命令添加 MongoDB 存储库:

```
cat > /etc/yum.repos.d/mongodb.repo << 'EOL'  [mongodb-org-4.4]  name=MongoDB Repository  baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.4/x86_64/  gpgcheck=1  enabled=1  gpgkey=https://www.mongodb.org/static/pgp/server-4.4.asc  EOL
```

现在可以通过运行以下命令来安装 MongoDB 的社区版本了:

```
sudo dnf install mongodb-org -y
```

通过运行以下命令启动并启用 MongoDB:

```
systemctl start mongod
```

```
systemctl enable mongod
```

请注意，您可以通过运行以下命令来检查 MongoDB 的安装状态:

```
mongod --version
```

### **如何在 Rocky Linux 上使用 MongoDB**

在上一节中，您学习了如何在 Rocky Linux 上安装 MongoDB。在下面，我们将教你如何实现和使用它。您可以通过运行以下命令来检查日志文件中的任何错误:

```
tail /var/log/mongodb/mongod.log
```

如果没有错误，您可以通过运行以下命令来访问 MongoDB shell:

```
mongo
```

要查看系统中可用的数据库，请运行以下命令:

```
db
```

#### **如何创建新的数据库和用户**

这里要创建一个新的 MongoDB 数据库，需要将 0 改为新的数据库。注意，我们将**创建一个名为 elder-db 的数据库**:

```
use elder-db
```

现在，您可以通过运行以下命令将一组文档(MongoDB 中的数据结构)插入到新数据库中。然后，按**键进入**:

```
db.linux.insertOne(  {"Rocky Linux" : "8",  "ubuntu" : "20.04",  "centos" : "8",  "debian" : "10"  }  )
```

现在，您可以使用以下命令来显示数据库中的文档:

```
show collections
```

您还可以使用以下命令来显示数据库的内容:

```
db.linux.find()
```

要将用户创建为具有读/写权限的管理员，可以使用以下命令。然后，按**键进入**:

```
db.createUser(  {  user: 'admin ',  pwd: '[[email protected]](/cdn-cgi/l/email-protection)',  roles: [ { role: 'readWrite', db: 'elder-db' } ]  }  );
```

如果您想查看用户列表，请运行以下命令:

```
db.getUsers()
```

最后，要退出 MongoDB，必须运行以下命令:

```
exit
```

## 结论

MongoDB 和一般的 NoSQL 数据库非常灵活，可以接受不同类型的数据，这对程序员来说是一个重要的优势。该数据库的可扩展性使其可以用于处理大数据的项目。在本文中，我们试图教您如何在 Rocky Linux 上设置和使用 MongoDB。如果你有任何问题，可以在评论区和我们分享。