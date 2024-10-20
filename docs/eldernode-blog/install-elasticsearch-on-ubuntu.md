# 教程在 Ubuntu 21.04 | 20.04 - Eldernode 博客上安装 Elasticsearch

> 原文：<https://blog.eldernode.com/install-elasticsearch-on-ubuntu/>

![Tutorial Install Elasticsearch on Ubuntu 21.04](img/9cae6b1b1c6887730712dff0ed792643.png)

Elasticsearch 是一个用 Java 开发的全功能开源搜索引擎。从各种来源获取非结构化数据，并将其存储为针对文本搜索进行了高度优化的复杂格式。在本文中，我们将教你如何在 Ubuntu 21.04 上安装 Elasticsearch。如果你想购买一台 [**Ubuntu VPS**](https://eldernode.com/ubuntu-vps/) 服务器，你可以访问 [Eldernode](https://eldernode.com/) 中提供的软件包。

## **如何在 Ubuntu 21.04 上安装 elastic search**

### **什么是 Elasticsearch？**

Elasticsearch 是一家名为 Elastic 的公司的产品，该公司成立于 2012 年。Elasticsearch 使用 Lucene Apache 作为索引和搜索的核心。Lucene 是一个非常复杂的库；但是不用担心，Elasticsearch 通过提供 API 可用性消除了所有的麻烦。

使用 Elasticsearch，可以快速有效地存储和分析大量数据。这在处理半结构化自然语言数据时特别有用。

弹性搜索的可扩展性和速度很高，可用于:

_ 搜索应用程序

_ 网站搜索

_ 组织搜索

_ 输入信息的分析

_ 检查程序性能

_ 数据的分析和可视化

_ 安全分析

_ 商业分析

### **在 Ubuntu 21.04 上安装 Elasticsearch 的先决条件**

在本节中，我们将向您展示在 [Ubuntu](https://blog.eldernode.com/tag/ubuntu/) 21.04 上安装 Elasticsearch 的先决条件。为此，只需遵循以下步骤。

您需要**更新包索引**。然后需要**安装 OpenJDK 8** :

```
apt update
```

```
apt install openjdk-8-jdk
```

现在，您可以使用以下命令检查 JAVA 状态:

```
java -version
```

## **在 Ubuntu 21.04 上安装 elastic search |****Ubuntu 20.04**

现在我们想教大家如何在 Ubuntu 21.04 上安装 Elasticsearch。只需按顺序正确执行以下步骤。

第一步是使用以下命令**为 Elasticsearch 包导入 GPG 键**:

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
```

然后，您需要使用以下命令**将 Elasticsearch 存储库**添加到系统中:

```
sh -c 'echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" > /etc/apt/sources.list.d/elastic-6.x.list'
```

完成上述步骤后，您必须首先使用以下命令**更新缓存**。然后**在系统上安装弹性搜索包**:

```
apt update
```

```
apt install elasticsearch
```

最后，您可以使用以下命令**启动**和**启用**elastic search 服务:

```
systemctl start elasticsearch.service
```

```
systemctl enable elasticsearch.service
```

### **如何在 Ubuntu 21.04 上配置 elastic search |****Ubuntu 20.04**

在本节中，我们想要检查 Elasticsearch 配置。为此，只需使用您想要的文本编辑器之一打开配置文件:

```
vi /etc/elasticsearch/elasticsearch.yml
```

现在您需要查看包含 **network.host** 的线性配置文件。然后**取消对**的注释，将其值改为 **0.0.0.0** 。注意，此时您需要将网络主机设置为 0.0.0.0。为此，请使用以下命令:

```
network.host: 0.0.0.0
```

有趣的是，如果你希望你的设备上的配置是私有的或本地的，你必须设置**网络.主机**到 **127.0.0.1** 。

在对配置文件进行了上述更改后，您现在可以**保存**文件并退出。现在，您需要使用以下命令重新启动**elastic search，以应用更改:**

```
systemctl restart elasticsearch
```

## 结论

使用 Elasticsearch 有很多好处。这些好处包括可伸缩性、速度、弹性搜索 API‌使用、多语言、文档优化、自动完成和免费模式。在本文中，我们试图一步一步地教你如何在 Ubuntu 21.04 上安装 Elasticsearch。