# IPv6 简介以及如何添加 CentOS Server - Eldernode

> 原文：<https://blog.eldernode.com/ipv6-and-how-to-add-on-centos/>

![Introducing IPv6 And How To Add On CentOS Server](img/3e6527d979c1b2c326c1ecee8c364102.png)

所有连接到互联网的设备，如电脑和手机，都需要一个数字 IP 地址来与其他设备进行通信。互联网协议版本 6 (IPv6)是互联网协议的最新版本。它为网络上的计算机提供一个识别和定位系统，并在互联网上路由流量。基本上，IPv6 旨在取代 IPv4，成为它的补充，并最终取代它。本文介绍了【IPv6 简介以及如何添加 CentOS 服务器。你可以在[老人节点](https://eldernode.com/)上找到你需要的东西。要购买您自己的 [CentOS VPS](https://eldernode.com/centos-vps/) ，您也可以向我们的技术团队寻求指导。值得一游！

## **介绍 IPv6**

首先，让我们更多地了解 Ipv6，并回顾一下**Ipv6 为什么重要**。由于设备分配了唯一的 IP 地址来识别和定位定义，因此需要更多的地址来连接设备。IPv4 是第一个公开使用的互联网协议版本，但其局限性导致 IETF 正式制定了后续协议。这正是 IPv6。 [IPv6](https://en.wikipedia.org/wiki/IPv6) 除了提供更大的寻址空间外，还提供了其他技术优势。IPv4 和 IPv6 在设计上并不具备互操作性。因此，它们之间的直接通信是不可能的，这使得向 IPv6 的迁移变得复杂。然而，已经设计了几种过渡机制来纠正这种情况。

### **IPv6 的好处**

使用 IPv6 可以让您体验更快的连接和更丰富的数据。由于一个不正确的信念，人们禁用 IPv6 来加快他们的互联网连接，但禁用不会这样做。然而，这可能是一个快速解决方案，但肯定会为以后积累问题。默认情况下，IPv6 在 [Windows](https://blog.eldernode.com/tag/windows/) 、 [Linux](https://blog.eldernode.com/tag/linux/) 和其他操作系统上是启用的，因为它们都内置了对 IPv6 的支持。你需要加入到 IPv6 的潮流中来。不仅禁用 IPv6 会导致问题，而且有必要用 IPv6 替换 IPv4。IPv6 的主要区别在于它使用 128 位 IP 地址(它可以支持 2^128 互联网地址)。

让我们回顾一下 **IPv6** **的优势和特性**:

1-不再有 [DHCP](https://blog.eldernode.com/analyze-dhcp-server-with-powershell/) ，更易于管理

2-不再需要网络地址转换

3-不再有私有地址冲突

4-内置认证和隐私支持

5-更简单的标题格式

6-更好的多播路由

7-无状态和有状态地址配置，自动配置

8-简化、更高效的路由

9-真正的服务质量(QoS)，也称为“流标签”

10-更大的地址空间

11-可扩展性

12-高效的分层寻址和路由基础设施

13-内置安全性

14-用于相邻节点交互的新协议

## **如何在 CentOS 服务器上添加 IPv6**

如您所知，开发 IPv6 是为了应对期待已久的 IPv4 耗尽问题。在本节中，您将了解如何在您的 [CentOS](https://blog.eldernode.com/tag/centos/) 服务器上添加 IPv6，以利用其高效的路由、定向数据流、简化的网络配置、安全性等等。

### **如何检查 IPv6 状态**

正如我们提到的，默认情况下，IPv6 通常在 Linux 上启用。但是，您可以通过两种方式检查您的 CentOS 系统是否启用了 Ipv6。

第一种方法通过运行以下命令向您展示:

```
sudo sysctl -a | grep ipv6.*disable
```

在输出中，值 0 表示 IPv6 在您的节点上处于活动状态，1 表示 IPv6 处于禁用状态。

检查 IPv6 是否启用的第二种方法是在 **/etc/network-scripts/** 目录下查看您的网络接口。所以。使用下面的命令:

```
cat /etc/sysconfig/network-scripts/ifcfg-enps03
```

### **IPV6 选项**

既然您将看到 IPv6 选项，让我们看看每个选项的含义:

**IPV6INIT=yes** :初始化 IPv6 寻址的接口。

**IPV6_AUTOCONF=yes** :启用接口的 IPV6 自动配置。

**IPV6_DEFROUTE=yes:** 这表示默认 IPV6 路由已经分配给接口。

**IPV6_FAILURE_FATAL=no:** 表示即使 IPV6 失败，系统也不会失败。

确保启用 IPv6 后，使用以下命令检查接口的 IPv6 地址:

```
ip a
```

运筹学

```
ip -6 addr
```

## **如何在 CentOS 上禁用和启用 IPv6**

您可以在您的服务器上暂时禁用 IPv6。为此，请使用以下命令:

```
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
```

```
ip -6 addr
```

由于任何原因，如果需要**永久禁用 IPv6】，编辑 GRUB **/etc/default/grub** 文件。在**行 GRUB_CMDLINE_LINUX** 中，追加参数 **ipv6.disable=1****

要应用更改，您需要**重启**您的系统。

为了**启用 IPv6** ，运行以下命令:

```
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=0
```

最后，您必须**重启**网络管理器以使更改生效:

```
sudo systemctl restart NetworkManager
```

## 结论

在本文中，我们向您介绍了 IPv6，并学习了如何在 CentOS 上添加 IPv6。现在你知道世界正在慢慢远离 IPv4。不建议您手动进行配置，因为 IPv6 的手动配置容易出错，而且相当费力。