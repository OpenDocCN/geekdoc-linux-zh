# MySQL 查询故障排除答案- Eldernode

> 原文：<https://blog.eldernode.com/mysql-queries-troubleshooting/>

![Answer To MySQL Queries Troubleshooting](img/ac9d5df44ed2340a2f23076d6861327e.png)

在下面的 MySQL 教程中，在这篇文章中，我们将回答 MySQL 查询故障排除。在您诊断 MySQL 设置时，加入我们的故障排除资源和起点。我们将回顾许多 MySQL 用户遇到的一些问题，并为解决具体问题提供指导。

为了让这个指南更好地发挥作用，尝试购买和使用 [VPS](https://eldernode.com/linux-vps/) 或 [Linux 虚拟主机](https://eldernode.com/linux-hosting/)。

## MySQL 查询故障排除答案

让我们回顾一下本指南，了解更多关于 MySQL 的信息。

作为用户，一旦开始对数据发出查询，您可能会遇到问题。在一些数据库系统中，包括 [MySQL](https://blog.eldernode.com/database-correction-with-database-management/) ，查询语句中必须以分号(T3)结尾； )进行查询完成，如下例:

```
SHOW * FROM table_name;
```

然后，如果您未能在查询末尾包含分号，提示将在新的一行继续，直到您通过输入分号并按下 **ENTER** 完成查询。

此外，一些用户可能会发现他们的查询非常慢。找出哪个查询语句导致速度变慢的一个方法是启用并查看 MySQL 的慢速查询日志。为此，打开您的 **mysqld.cnf** 文件，该文件用于配置 MySQL 服务器的选项。该文件通常存储在 **/etc/mysql/mysql.conf.d/** 目录下:

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

滚动浏览文件，直到看到以下几行:

/etc/MySQL/MySQL。conf 文件。d/mysqld。法国国家足球队

```
. . .  #slow_query_log         = 1  #slow_query_log_file    = /var/log/mysql/mysql-slow.log  #long_query_time = 2  #log-queries-not-using-indexes  . . .
```

看看这些注释掉的指令，它们为慢速查询日志提供了 MySQL 的默认配置选项。具体来说，他们每个人都做了以下工作:

**慢速查询日志**:设置为 1 启用慢速查询日志。

**慢速查询日志文件**:这定义了 MySQL 将记录任何慢速查询的文件。在本例中，它指向/var/log/mysql-slow.log 文件

**long_query_time** :通过将该指令设置为 2，它将 MySQL 配置为记录任何耗时超过 2 秒的查询。

**log _ queries _ not _ using _ indexes**:这告诉 MySQL 也将任何不带索引运行的查询记录到/var/log/mysql-slow.log 文件中。此设置不是慢速查询日志运行所必需的，但它有助于发现低效的查询。

通过删除前导井号( **#** )来取消对这些行的注释。该部分现在将如下所示:

/etc/MySQL/MySQL。conf 文件。d/mysqld。法国国家足球队

```
. . .  slow_query_log = 1  slow_query_log_file = /var/log/mysql-slow.log  long_query_time = 2  log_queries_not_using_indexes  . . .
```

**注意 :** 如果运行的是 **MySQL** 8+，默认情况下这些注释行不会在 **mysqld.cnf** 文件中。在这种情况下，将下列行添加到文件的底部:

/etc/MySQL/MySQL。conf 文件。d/mysqld。法国国家足球队

```
. . .  slow_query_log = 1  slow_query_log_file = /var/log/mysql-slow.log  long_query_time = 2  log_queries_not_using_indexes
```

当您启用慢速查询日志时，可以保存并关闭该文件。然后重启 **MySQL** 服务:

```
sudo systemctl restart mysql
```

有了这些设置，您可以通过查看慢速查询日志来找到有问题的查询语句。你可以少用 **，** 这样做:

```
sudo less /var/log/mysql_slow.log
```

此外，MySQL 包括 **解释** 语句，该语句提供了关于 MySQL 如何执行查询的信息。【MySQL 官方文档中的这一页提供了关于如何使用 **解释** 来突出低效查询的见解。

## 结论