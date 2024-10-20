# 如何在 Ubuntu 20.04 上安装和配置 oVirt-elder node 博客

> 原文：<https://blog.eldernode.com/install-and-configure-ovirt-on-ubuntu-20-04/>

![How to Install and Configure oVirt on Ubuntu 20.04](img/46b7fa83597ba26c0143f2cc1ea22dbb.png)

oVirt 是一个免费的开源虚拟化管理平台。oVirt 旨在管理虚拟机、计算、存储、整个企业基础架构和网络资源。oVirt 提供虚拟机和磁盘在主机和存储之间的实时迁移。它还使用基于内核的虚拟机(KVM ),并像一些社区项目一样构建在 libvirt、Gluster、PattenFly 和 Ansible 之上。加入我们这篇文章，学习如何**在 Ubuntu 20.04** 上安装和配置 oVirt。不要错过 [Eldernode](https://eldernode.com/) 的 2021 年热卖优惠，购买最合适的套餐。购买您自己的 [Linux VPS](https://eldernode.com/linux-vps/) 并和我们在一起！

为了让本教程更好地发挥作用，请考虑以下**先决条件**:

拥有 sudo 权限的非 root 用户。

要进行设置，请遵循我们在 Ubuntu 20.04 上的[初始服务器设置。](https://blog.eldernode.com/initial-server-setup-on-ubuntu-20/)

## **教程在 Ubuntu 20.04 上安装和配置 oVirt**

oVirt 拥有 GPLV2 许可证。oVirt 引擎后端用 Java 编写，前端用 GWT web toolkit 开发。oVirt 由两个基本组件组成，oVirt 引擎和 oVirt 节点。在 [Ubuntu](https://blog.eldernode.com/tag/ubuntu/) 上安装 oVirt 的过程真的比你想象的要简单。

oVirt 访客代理提供 oVirt web 界面和访客之间的信息、通知和操作。代理向 web 界面提供机器名、操作系统、IP 地址、安装的应用程序、网络和 RAM 使用情况。它还提供了单点登录，因此通过 web 界面身份验证的用户在连接到虚拟机时无需再次进行身份验证。

### **在 Ubuntu 20.04 上安装 oVirt 访客代理|** **Ubuntu 18.04**

推荐你从其官网[下载](https://www.ovirt.org/download/) oVirt。

首先，通过运行以下命令更新您的系统:

```
sudo apt -y update
```

然后，要安装 oVirt-agent，请在终端中键入以下命令:

```
sudo apt-get install -y ovirt-guest-agent
```

这样就可以在你的 Ubuntu 上成功安装 oVirt 了。

### **如何在 Ubuntu 上配置 oVirt**

因为你使用的是 Ubuntu，服务会自动被**配置为**启动。

![oVirt Dashboard](img/c8dc6d8dee3486fa5e6ded19d38f46fe.png)

## **如何卸载 oVirt-guest-agent**

如果您希望从 Ubuntu 20.04 中卸载 oVirt，您可以使用以下命令删除 ovirt-guest-agent 包本身。

```
sudo apt-get remove ovirt-guest-agent
```

此外，要卸载 ovirt-guest-agent 及其依赖项，请运行:

```
sudo apt-get remove --auto-remove ovirt-guest-agent
```

但是如果您需要删除 ovirt-guest-agent 的本地/配置文件，请使用下面的命令。注意，与上面两个相反，被清除的配置/数据**不能通过重新安装软件包来恢复。**

```
sudo apt-get purge ovirt-guest-agent
```

运筹学

```
sudo apt-get purge --auto-remove ovirt-guest-agent
```

## **结论**

在本文中，您了解了如何在 Ubuntu 20.04 上安装和配置 oVirt。虚拟机在主机出现故障时可用。您将拥有对主机、存储和存储的集成管理。如果你有兴趣了解更多关于 Ubuntu 的知识，找到你需要的科目，并在[社区](https://community.eldernode.com/)上与你的朋友取得联系。