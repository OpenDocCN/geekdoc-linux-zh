# 教程在 Debian 11 上配置 Apache Cassandra 牛眼]

> 原文：<https://blog.eldernode.com/configure-apache-cassandra-on-debian-11/>

![Tutorial Configure Apache Cassandra on Debian 11 [Bullseye]](img/9680d3b9a47e77b9d1aa86082c17e815.png)

Apache Cassandra 是一个开放源代码的分布式 NoSQL 数据库，因其可伸缩性和高可用性而受到成千上万家公司的信任，同时又不影响性能。商品硬件或云基础设施上的线性可扩展性和经验证的容错能力使其成为任务关键型数据的绝佳平台。在本文中，我们将一步步教你如何在 Debian 11 上安装和配置 Apache Cassandra。另外，如果你想购买一个 [**Linux VPS**](https://eldernode.com/linux-vps/) 主机，你可以访问 [Eldernode](https://eldernode.com/) 中的软件包。

## **如何安装&在 Debian Linux 上配置 Apache Cassandra**

Apache Cassandra 4.0 是该项目历史上最稳定的版本，是向 12 个月发布周期转变的开始，支持 3 年的发布期。最新版本经过了严格的测试，为分布式数据库设立了新的高基准，并包括以下新功能:提高了速度和可伸缩性、改进了一致性、新的配置设置、最小化了延迟、增强了安全性和可观察性、更好的压缩。

### 阿帕奇卡珊德拉功能:

*   混合物
*   容错
*   关注质量
*   表演的
*   你在控制中
*   安全性和可观察性
*   分布的
*   可攀登的
*   弹性的

## 先决条件

*   安装最新版本的 Java 8。
*   要使用 cqlsh，请安装最新版本的 Python 2.7 或 Python 3.6+。

## **在 Debian 11 上安装 Apache Cassandra**

### A)在 Debian 上安装 Java

要运行 Apache Cassandra，你必须首先在你的 Debian linux 服务器上安装 Java。

```
sudo apt install openjdk-11-jdk -y
```

使用以下命令检查安装的 Java 版本。

```
java -version
```

### B)安装 Apache Cassandra 库

```
sudo apt install apt-transport-https gnupg2 -y
```

```
sudo wget -q -O - https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -
```

```
sudo sh -c 'echo "deb https://downloads.apache.org/cassandra/debian 40x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list'
```

### 安装 Apache Cassandra

首先，更新你的 Debian 11 Linux 服务器。

```
sudo apt update
```

然后在您的 Debian 服务器上运行以下命令。

```
sudo apt install cassandra -y
```

```
sudo systemctl enable cassandra
```

```
sudo systemctl status cassandra
```

```
sudo nodetool status
```

### 配置 Apache Cassandra

要使用 cqlsh 命令行工具与 Cassandra 集群进行交互，请登录。

```
cqlsh
```

完成后，退出命令。

```
EXIT;
```

## 结论

恭喜你，你可以在你的 Linux Debian 11 服务器上安装 Apache Cassandra 了。您也可以从[官方网站](https://cassandra.apache.org/doc/latest/)上阅读文档以进行进一步检查。