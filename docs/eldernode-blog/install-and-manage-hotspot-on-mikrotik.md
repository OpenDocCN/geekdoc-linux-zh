# 如何在 Mikrotik - Eldernode 博客上安装和管理 Hotspot

> 原文：<https://blog.eldernode.com/install-and-manage-hotspot-on-mikrotik/>

![How To Install And Managing Hotspot on Mikrotik](img/7916224b8ffcbec816c0d8867440c818.png)

之前，您已经阅读了关于 Mikrotik 的各种相关主题。无需登录或启动，您就可以使用 Mikrotik 作为强大的路由器来提高启动速度并提供自动性能。您可以在计算机和服务上安装基于 Linux 内核的 Mikrotik 服务器。在本文中，您将学习如何在 Mikrotik 上安装和管理 Hotspot。此外，为了提供一个 [Mikrotik VPS](https://eldernode.com/mikrotik-vps-server/) 服务器， [Eldernode](https://eldernode.com/) 的软件包可以提供最优惠的价格和支持。

## **教程在 Mikrotik** 上安装和管理热点

加入我们来看看什么是 Hotspot 以及它在路由器上的配置和设置方式。您可以为任何环境设置热点。请注意，如果您使用向导模式来设置热点以保持对路由器的访问，效果会更好。在本指南的以下内容中，您将阅读 TLS 证书的描述。此外，您还会发现为什么要定义 SMTP 服务器和 T2 DNS 服务器，以及应该使用什么结构来定义 DNS 名称。要查看热点激活后动态创建的部分，请继续阅读，直到结束。

### **如何设置热点服务器**

在这一部分中，您将学习如何使用不同的配置文件来设置 Hotspot 服务器。

首先，您应该配置连接到 WAN 的接口。

```
/ ip address add address=192.168.1.5/24 network=192.168.1.0 broadcast=192.168.1.255 interface=ether1
```

当一个接口配置并连接到 WAN 时，您可以为本地网络配置第二个接口。

```
/ ip address add address=10.10.0.1/24 network=10.10.0.0 broadcast=10.10.0.255 interface=ether2
```

现在，您的两个接口都已配置好并正常工作。因此，您可以将您的 [Mikrotik](https://blog.eldernode.com/tag/mikrotik/) ROS 设置为 DNS 服务器，因为这是设置 DNS 服务器的一个非常好的实践。

### **如何在 Mikrotik** 上配置热点

很明显，我们使用 Hotspot 快速方便地访问互联网。要访问热点，请导航至 IP >热点。

![Hotspot menu](img/5ab9403f63d146c47e858722ca599682.png)

当您选择热点设置而不是手动模式时，它有助于您确保保持访问[路由器](https://blog.eldernode.com/static-route-in-mikrotik/)，因为您不会错过任何重要步骤。

首先，你将被要求提供你要在其上设置热点的界面。因此，选择您的内部网络接口或本地接口。

![Hotspot interface](img/ad0b53fc1d063dd564be779488ed09a6.png)

接下来，您应该输入网络的本地地址，默认情况下将本地接口[设置为 IP](https://blog.eldernode.com/ip-settings-in-mikrotik/) 。这里我们不改变它。此外，如果你喜欢伪装网络，标记立方体。

![Local Address of the Network](img/4c7bc55017551649818e4aca24c1a037.png)

现在，要求您为热点地址设置地址池。

![Hotspot address pool](img/9a435361809ebe389f1da2c8e53279d6.png)

在此步骤中，如果您希望通过 HTTPS 保护热点，您需要 TLS 证书。如果您有 TLS 证书，请添加它，如下所示:

![hotspot SSL certificate](img/33f23b65b3df6ee9c765d83ce68c00ed.png)

然后，您必须添加 SMTP 服务器的 IP 地址。如果您的网络上有 SMTP 服务器或邮件服务器，请添加地址，以便在您考虑的 SMTP 服务器上重定向所有电子邮件请求。

![SMTP server](img/4d08073bf7f88f16a656157935bd48f7.png)

接下来，需要 DNS 服务器。DNS 服务器将被分配给你的热点客户端。

![DNS configuration](img/cb39563505edf07f908899927413ca7c.png)

在这一步中，您将被要求输入 DNS 名称。热点 DNS 名称应该具有 FQDN 结构。

注意 FQDN 结构是由一个点分隔的两部分。例如:Eldernode.local

![DNS name of local hotspot server](img/851ccdb9a2a1eb33467410e248b24c73.png)

毕竟，您将添加您的第一个本地热点用户的名称。你可以选择你想要的任何东西。默认情况下，名称为“admin ”,没有密码。

![Create local Hotspot user](img/28f3f034792c0f8512318b5340bcbcc3.png)

通过点击下一步按钮，热点将被创建。

![Hotspot is created](img/a184ad1edf8124b6a0f43a7bc35027de.png)

一旦热点被激活，你将不再访问互联网。因此，如果您选中它，热点登录页面将会打开。

![Hotspot login page](img/5355557fbf3e5f9b737654d45dfc46f1.png)

换句话说，当热点被创建时，用户通过热点界面的访问将会丢失。他们需要被认证才能再次访问互联网。

![Hotspot ](img/d3bfc45a2554397c41e1b5bfd8582e09.png)

由于热点正在创建，一些部分正在添加到设置，您可以从 IP 选项导航。

IP 池是动态添加的，您可以在其中查看您最近选择的网络的 IP 池地址范围。

![IP pool](img/cafc9a5cd418ece0b7c61cb06185f777.png)

另一个额外的部分是“防火墙”，一些规则动态地添加到过滤规则和 NAT 选项卡中。

![firewall rules](img/11b7c689bcadf8728d0fe60d75a07617.png)

在服务器配置文件选项卡中，正在为热点创建配置文件。

![Hotspot server profile](img/16a341f5f4883741eefe9dc1b23dfe1a.png)

“用户”选项卡包含用户的名称。如果您在相关步骤中添加了姓名，您可以在此处查看。

![Hotspot user admin](img/05752b084ef57649c6c04745e8a53e8f.png)

## 结论

在本文中，您了解了如何在 Mikrotik 上安装和管理 Hotspot。要了解更多信息，请找到我们关于如何更新和备份 MikroTik 的相关文章[。](https://blog.eldernode.com/how-to-update-and-backup-mikrotik/)