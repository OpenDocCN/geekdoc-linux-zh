# Kali Linux - Eldernode 博客介绍并安装 ace-voip

> 原文：<https://blog.eldernode.com/introducing-and-install-ace-voip-on-kali/>

![Introducing And Install ace-voip On Kali Linux](img/789813f7927f94d8a5411de3a853cb59.png)

**A** 自动 **C** 公司 **E** 分子是一个简单而强大的 VoIP 公司目录枚举工具。它充当 IP 电话来下载给定电话可以在其屏幕界面上显示的名称和分机条目。ACE 是作为一种研究思路从“VoIP Hopper”发展而来的。它使针对企业目录中的名称的 VoIP 攻击自动化。攻击将根据用户的名字对他们进行，而不是针对随机 RTP 音频流或 IP 地址的 VoIP 流量。加入我们这篇文章来回顾一下**在 Kali Linux 上介绍和安装 Ace-VOIP**。要购买您自己的 [Linux VPS](https://eldernode.com/linux-vps/) ，请访问 [Eldernode](https://eldernode.com/) 上提供的软件包，感受其中的差异。

## **介绍卡莉 Linux 上的 Ace-VOIP**

在本文中，我们尝试在第一步向您介绍 Ace-VOIP 工具以及 VOIP 的优缺点。然后我们教你如何在 [Kali Linux](https://blog.eldernode.com/install-and-configure-kali-linux-on-vps/) 上安装 Ace-VOIP。请加入我们。

### **什么是 ACE-VOIP 查号工具？**

Ace-VOIP 拥有 GPLv3 的许可。VoIP 是一组用于在互联网协议网络上传送语音通信和多媒体会话的技术。为了下载 VoIP corporate，ACE 使用 DHCP、TFTP 和 HTTP。它的工作原理是将目录输出到一个文本文件中，该文件可以用作其他 VoIP 评估工具的输入。如果您需要通过降低线路租金和通话成本来为企业节省大量成本，那么使用 Ace-VoIP 可能是一个不错的解决方案。

由于小型和企业用基于 IP 的电话系统取代了他们的旧的传统电话系统，他们将拥有基于 VoIP 的 PBX 的各种功能。

您可以通过以下两种方式之一使用 ACE:

1-自动通过 [DHCP](https://blog.eldernode.com/analyze-dhcp-server-with-powershell/) 发现 TFTP 服务器 IP 地址。

2-将 TFTP 服务器 IP 地址指定为工具的命令行参数。

***注意* :** 要正确下载配置文件，需要提供带有-m 选项的 IP 电话的 [MAC 地址](https://blog.eldernode.com/change-the-mac-address-in-windows/)(必填，受害者 IP 电话的 MAC 地址)。

#### **VOIP 优势(Kali 上介绍并安装 ace-VOIP)**

以下是 VoIP 优势的列表:

1.  降低成本
2.  增加可访问性
3.  完全可移植性
4.  更高的可扩展性
5.  小型和大型团队的高级功能
6.  更清晰的音质
7.  支持多任务处理
8.  软电话更灵活

#### **网络电话缺点**

在任何工具中发现一些缺点都是正常的。让我们来看看网络电话的缺点列表。

1-需要可靠的互联网连接

2-延迟和抖动

3-紧急呼叫没有位置跟踪

#### **【ACE 支持(在 Kali Linux 上引入并安装 ACE-VoIP)**

ACE 支持思科统一 IP 电话中使用的 VoIP 公司目录。它是这样工作的:

1.  欺骗 CDP 得到 VVID
2.  添加语音 VLAN 接口(VLAN 跳)–后续流量标记为 VVID
3.  发送标有 VVID 的 DHCP 请求
4.  通过 DHCP 选项 150 解码 TFTP 服务器 IP 地址
5.  发送对 IP 电话配置文件的 TFTP 请求
6.  解析文件，学习公司目录 URL
7.  发送对目录的 HTTP GET 请求
8.  解析 XML 数据，将目录用户写入格式化的文本文件

### **如何在 Kali Linux 上安装 Ace-VOIP**

如你所知，在 Kali 上安装[工具的过程简单快捷。运行以下命令在 Kali Linux 上安装 Ace-VOIP。](https://blog.eldernode.com/tag/kali-linux/)

```
sudo apt-get install ace-voip
```

这样，您将安装 Ace-VOIP 及其依赖包。要安装其依赖项，请键入:

```
sudo apt-get install libc6
```

```
sudo apt-get install libpcap0.8
```

## 结论

在本文中，向您介绍了 Ace-VOIP，您学习了如何在 Kali Linux 上安装 Ace-VOIP。从现在开始，你可以忘记铜质无线手机，使用联网电脑、耳机和 VoIP 随时随地打电话。它将您的声音转换成数字信号，让您可以直接从计算机、VoIP 电话或任何数据驱动设备拨打电话。如果你有兴趣了解更多，可以在 [Kali Linux 教程](https://blog.eldernode.com/tag/kali-linux/)上找到更多相关文章。