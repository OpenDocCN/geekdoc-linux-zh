# 教程在 Ubuntu 20.04 上安装 Osquery LTS-elder node 博客

> 原文：<https://blog.eldernode.com/install-osquery-on-ubuntu-20-04/>

![Tutorial Install Osquery on Ubuntu 20.04 LTS](img/dd4d7b6dcf84c82e282e608f79130276.png)

由脸书开发的 Osquery 给用户带来了所用硬件的列表。这个工具乍一看可能不太有趣，但它有很多用途。它可用于查看通过 USB 连接的硬件列表。Osquery 可以在不使用低级函数或任何 API 的情况下与操作系统通信。事实上，Osquery 对于那些想要保护他们的应用程序免受安全破坏或者监控他们在不同系统上的性能的开发人员来说非常有用。在这篇文章中，我们试图学习如何在 Ubuntu 20.04 LTS 上安装 Osquery。你可以在 [Eldernode](https://eldernode.com/) 看到购买 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) 服务器的套餐。

## **如何在 Ubuntu 20.04 上安装 Osquery LTS**

Osquery 是开源的跨平台软件，用于将操作系统表示为关系数据库。您可以使用 Osquery 来执行基于 SQL 的查询，以便从操作系统中检索数据。Osquery 是一个将操作系统显示为高性能关系数据库的工具。该工具使程序员能够编写基于 SQL 的查询来探索操作系统信息。有趣的是，使用 Osquery，可以创建 SQL 表来表示抽象概念，例如:

–硬件事件

–运行流程

–文件哈希

–加载的内核模块

–浏览器插件

–开放网络连接

关注本文，了解如何在 Ubuntu 上安装 Osquery。

### **在 Ubuntu 20.04 上安装 Osquery | Ubuntu 18.04**

由于 Osquery 包不在 Ubuntu 默认的存储库中，所以您必须在安装之前添加 Osquery apt 存储库。为此，您可以使用以下命令:

```
echo "deb [arch=amd64] https://pkg.osquery.io/deb deb main" | sudo tee /etc/apt/sources.list.d/osquery.list
```

然后，您必须通过执行以下命令来导入存储库签名密钥:

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 1484120AC4E9F8A1A577AEEE97A80C63C9D8B80B
```

执行上述命令后，您现在必须重新启动系统一次:

```
sudo apt-get update
```

在这一步中，您可以通过执行以下命令来安装 Osquery:

```
sudo apt-get install osquery
```

需要注意的是，安装 Osquery 后，您可以使用下面的命令找到正确的安装:

```
osqueryi --version
```

### **如何在 Ubuntu 上使用 Osquery**

如果您正确地遵循了这些步骤，现在您可以通过运行以下命令轻松地运行 Osquery:

```
osqueryi
```

## 结论

如前所述，Osquery 是一个用于 Windows、OS X (macOS)、Linux 和 FreeBSD 的操作系统框架。这些工具使低级操作系统监控既实用又直观。Osquery 可以将操作系统表示为高性能的关系数据库。这允许您编写 SQL 查询来浏览操作系统数据。还应该注意，SQL 表使用抽象概念，如运行的进程、加载的内核模块、开放的网络连接、浏览器插件、硬件事件或使用 Osquery 的文件散列。在这篇文章中，我们试图向你学习如何在 Ubuntu 20.04 LTS 版上安装 Osquery。