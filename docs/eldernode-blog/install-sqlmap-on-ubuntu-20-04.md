# 如何在 Ubuntu 20.04 LTS 上安装 SQLMap-[运行 SQLMap]

> 原文：<https://blog.eldernode.com/install-sqlmap-on-ubuntu-20-04/>

![Tutorial-Install-and-Use-SQLMap-on-Ubuntu-20.04](img/1bf7a585ff7e9617f4c18af22d8b0a93.png)

SQL 注入攻击是网络世界中的一种攻击，它在 URL 中注入 SQL 代码并执行所需的命令。最著名的 SQL 注入攻击之一是 SQLMap。我们将在本文中介绍 SQLMap，然后您将学习如何在 Ubuntu 20.04 LTS 上**安装和使用 SQLMap。你可以在 [Eldernode](https://eldernode.com/) 网站上查看并[购买 linux vps](https://eldernode.com/linux-vps/) 服务器包。**

## 如何在 Ubuntu 20 上运行 SQLMap

### **什么是 SQLMap？**

SQLMap 是一个开源渗透测试工具，它可以自动识别和利用 SQL 注入漏洞，并接管数据库服务器。该设备具有许多特殊功能，如强大的检测引擎、数据库指纹识别、从数据库获取数据、访问底层文件系统以及通过带外连接在操作系统上执行命令。

在这篇来自 [Ubuntu 培训](https://blog.eldernode.com/tag/ubuntu/)系列的文章的续篇中，我们打算教你如何在 Ubuntu 20.04 上安装和使用 SQLMap。

## **在 Ubuntu 20.04 上安装 SQLMap**

在开始安装之前，**使用以下命令更新包**存储库:

```
sudo apt-get update
```

然后**通过运行以下命令安装 SQLMap** :

```
sudo apt-get install sqlmap
```

最后，检查系统日志，确保没有相关的错误。

### **如何在 Ubuntu 20 上使用 SQLMap**

以下命令显示**基本帮助信息**:

```
sqlmap -h
```

```
sqlmap --help
```

以下命令显示**高级帮助信息**:

```
sqlmap -hh
```

下面的命令显示了程序的**版本号**:

```
sqlmap --version
```

运行以下命令**定义目标**:

```
sqlmap -u URL
```

使用以下命令指定**如何连接到目标 URL** :

```
sqlmap --data
```

```
sqlmap --cookie
```

```
sqlmap --random-agent
```

```
sqlmap --proxy
```

```
sqlmap --tor
```

```
sqlmap --check-tor
```

运行以下命令来指定要测试的参数，提供自定义注入负载和可选的篡改脚本:

```
sqlmap -p
```

```
sqlmap --dbms
```

使用以下命令**定制检测阶段**:

```
sqlmap --level
```

```
sqlmap --risk
```

运行下面的命令来调整特定 SQL 注入技术的测试:

```
sqlmap --technique
```

您可以使用以下命令枚举表中包含的后端数据库管理系统信息、结构和数据:

```
sqlmap --all
```

```
sqlmap --banner
```

```
sqlmap --current-user
```

```
sqlmap --current-db
```

```
sqlmap --passwords
```

```
sqlmap --tables
```

```
sqlmap --columns
```

```
sqlmap --schema
```

```
sqlmap --dump
```

```
sqlmap --dump-all
```

```
sqlmap -D DB
```

```
sqlmap -T TBL
```

```
sqlmap -C COL
```

运行以下命令**访问后端数据库管理系统**底层操作系统:

```
sqlmap --os-shell
```

```
sqlmap --os-pwn
```

使用以下命令设置**通用工作参数**:

```
sqlmap --batch
```

```
sqlmap --flush-session
```

## 结论

在本文中，我们介绍了最著名的 [SQL](https://blog.eldernode.com/sql-server-services/) 注入攻击之一。SQLMap 是一个开源渗透测试工具。这样，你就学会了如何在 Ubuntu 20.04 上安装和使用 SQLMap。