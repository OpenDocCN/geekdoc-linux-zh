# Ubuntu 22.04 - Eldernode 博客上的 Neo4j 安装教程

> 原文：<https://blog.eldernode.com/setup-neo4j-on-ubuntu-22-04/>

![Tutorial Setup Neo4j on Ubuntu 22.04](img/0de0eb1ac7c411d29f957a7a1b64fb4d.png)

Neo4j 是一个基本的图形数据库，被归类为非 sql 数据库。高速和运行复杂查询的能力是这个数据库最突出的特点。Neo4j 的最佳用途是将其作为其他数据库(如 Mongo 和 Cassandra)的补充。在本文中，我们将一步一步地教你如何在 Ubuntu 22.04 上设置 Neo4j。如果你想购买一台 [**Ubuntu VPS**](https://eldernode.com/ubuntu-vps/) 服务器，你也可以查看 [Eldernode](https://eldernode.com/) 网站上提供的软件包。

## **如何在 Ubuntu Linux 上配置 Neo4j**

### **Neo4j 及其特点介绍**

Neo4j 是一个新的数据库(NoSQL)数据库，它使用一个图形数据模型来存储数据并在它们之间进行通信。也就是说，它以图形的形式存储所有数据，并作为一组节点和它们之间的关系。因此，与其他关系数据库相比，不需要编写复杂的连接，各种类型的查询可以通过在图中导航和移动来轻松完成。

如前所述，Neo4j 最好作为 Mongo、Cassandra 等其他数据库的补充；原始数据存储在 Mongo 或 Cassandra 数据库中，这些数据的索引与查询中广泛使用的一些关键参数一起存储在 Neo4j 中。

大多数查询在 Neo4j 上运行，并使用找到的键从原始数据库中检索原始数据。这种方法广泛应用于非常大的项目(比如社交网络)。ElasticSearch 也是如此。

在 Neo4j 中使用图表代替表格，除了更少更简单的编码之外，还有助于形成一个完全敏捷和灵活的系统来存储各种数据和关系。此外，由于使用了图表，对数据和数据之间的关系有了直观的了解，即使对于非专家来说，分析和理解项目也将变得更加容易。

跟随我们这篇文章，向您展示如何在 [Ubuntu](https://blog.eldernode.com/tag/ubuntu/) 22.04 上安装 Neo4j。

### **在 Ubuntu 22.04 上安装 Neo4j 的先决条件**

->非超级用户访问

->[Ubuntu 22.04 上的初始服务器设置](https://blog.eldernode.com/initial-server-setup-on-ubuntu-22-04/)

## **如何在 Ubuntu 22.04 上安装 Neo4j**

在我们开始在 Ubuntu 22.04 上安装 Neo4j 之前，你应该知道在官方的 Ubuntu 软件包库中没有 Neo4j 数据库引擎的副本。因此，必须使用以下命令添加 Neo4j 中的 **GPG 键**:

```
curl -fsSL https://debian.neo4j.com/neotechnology.gpg.key |sudo gpg --dearmor -o /usr/share/keyrings/neo4j.gpg
```

现在，您需要通过运行以下命令将 Neo4j 4.1 存储库添加到您的系统 APT 资源中:

```
echo "deb [signed-by=/usr/share/keyrings/neo4j.gpg] https://debian.neo4j.com stable 4.1" | sudo tee -a /etc/apt/sources.list.d/neo4j.list
```

现在您已经成功地添加了 Neo4j 4.1 存储库，您需要使用下面的命令更新您的数据包列表。

```
sudo apt update
```

在通过运行以下命令更新软件包后，还需要安装 Neo4j 软件包及其所有依赖项:

```
sudo apt install neo4j
```

在此步骤中，您必须通过执行以下命令来**启用 Neo4j** 作为服务:

```
sudo systemctl enable neo4j.service
```

启用 neo4j 服务后，您现在需要使用以下命令**启动**它:

```
sudo systemctl start neo4j.service
```

您现在可以检查 neo4j 服务的**状态**:

```
sudo systemctl status neo4j.service
```

### **在 Ubuntu 22.04 上安装 Neo4j**

正如您在上一节中看到的，您已经学会了如何在 Ubuntu 22.04 上安装 neo4j。在本节中，我们将向您展示如何连接、配置和设置 Neo4j。请继续关注本文的其余部分。

要在命令行上与 Neo4j 交互，您可以首先使用以下命令并调用它:

```
cypher-shell
```

首次登录后，您需要更改管理员密码。注意，你默认的**密码**和**用户名**是 **neo4j** 。

成功设置管理员密码并建立连接后，您现在可以通过运行以下命令退出:

```
:exit
```

在本文的剩余部分，我们将向您展示如何配置 Neo4j 以允许远程连接。要进行所需的设置，您需要使用所需的文本编辑器打开配置文件。

```
sudo nano /etc/neo4j/neo4j.conf
```

现在你必须按下 **ctrl+w** 来搜索**# DBMS . default _ listen _ address = 0 . 0 . 0 . 0**，并通过删除 **#** 字符来取消注释:

这里有趣的是，你应该默认考虑将 **Neo4j 0.0.0.0** 用于你系统中的所有 IPv4 接口，包括 localhost。这意味着如果你想把 Neo4j 限制在一个**特定的 IP 地址**，你必须配置那个 IP 地址。

在配置文件中完成所需设置后，按下“ **Ctrl+X** ”，然后按下“ **Y** ，保存设置。

另一方面，需要注意的是，如果你想用 IPv6 地址配置 Neo4j，你必须配置一个 **DNS 名称**来解析到 IPv6 地址。另一种方法是在远程系统的 **/etc/hosts** 文件中添加一个条目。这样做将导致地址被映射到一个名称。一旦您正确地完成了这些，您就可以使用 DNS 文件名或主机来连接 Neo4j。

### **如何连接服务器**

考虑下面的例子。在下面的例子中，一个具有 **IPv6** 地址的 **Neo4j 服务器**需要一个远程连接系统，该系统具有如下的 **/etc/host** 输入，并用 **your_hostname** 替换一个突出的名称:

```
2001:db8::1 your_hostname
```

现在，您可以使用以下命令远程连接到服务器:

```
cypher-shell -a 'neo4j://your_hostname:7687'
```

如果您想要限制 Neo4j 使用**:::1**的 IPv6 **localhost** 地址，也可以执行以下操作，如以下命令所示:

```
cypher-shell -a 'neo4j://ip6-localhost:7687'
```

### **防火墙设置为 Neo4j**

要使用**端口 7687** 配置防火墙以允许访问远程主机，您需要运行以下命令:

```
sudo ufw allow from 123.4.567.8 to any port 7687 proto tcp
```

***注 1:*** 在命令中，您应该替换您用来访问 Neo4j 的远程系统的 IP 地址，而不是写入的 IP 地址。

您可以使用以下命令访问整个网络区域:

```
sudo ufw allow from 192.0.2.0/24 to any port 7687 proto tcp
```

***注 2:*** 输入你要访问 Neo4j 的网络的 IP，而不是上面命令中输入的 IP。

另一种可能是您希望允许主机远程访问 Neo4j。为此，您需要使用以下命令:

```
sudo ufw allow from 2001:DB8::1/128 to any port 7687 proto tcp
```

***注 3:*** 在上面的命令中，你必须替换你想要的系统的 IPv6 地址。

要允许**访问 IPv6 地址**，您需要在第一步运行以下命令:

```
sudo ufw allow from 2001:DB8::/32 to any port 7687 proto tcp
```

***注 4:*** 用 1234: 567 :: / 89 替换上面命令中你想要的网络范围。

您现在必须**使用以下命令启用 UFW** 来应用规则:

```
sudo ufw reload
```

要检查 UFW 规则的**状态，您可以运行以下命令:**

```
sudo ufw status
```

## 结论

在一些应用和网站中，除了数据本身，存储数据之间的联系也是非常重要的。例如，在社交网络中，需要确定一个人与其他哪些成员有关系。无论是像 Ebay 这样的购物网站还是像 Google 这样的搜索引擎，都需要维护对象和页面之间的关系，这种关系会随着时间的推移而发展。在这种类型的应用程序中，数据之间的关系甚至比数据本身更有价值(关系优先方法)。Neo4j 是一个新的数据库(NoSQL 数据库),使这一点变得很容易。在本文中，我们试图教你如何在 Ubuntu 22.04 上设置 Neo4j。