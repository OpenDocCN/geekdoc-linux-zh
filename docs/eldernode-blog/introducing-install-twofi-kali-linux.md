# Kali Linux - Eldernode 博客介绍及如何安装 Twofi

> 原文：<https://blog.eldernode.com/introducing-install-twofi-kali-linux/>

![Introducing And How To Install Twofi On Kali Linux](img/a1b7bf819d775a8706323c70407bcbff.png)

当然，我们都经历过这种情况，我们花了几个小时在互联网上，并猜测它应该只过了 5 分钟。因此，我们需要一个解决方案来避免错过黄金时间和滥用信息。开源情报(OSINT)帮助我们从公共来源收集和关联信息，以便在情报环境中使用。当你需要访问与网上某些主题相关的信息时，你开始搜索和分析，这可能要花费很多时间。显然，我们需要智能工具将漫长的过程缩短到几秒钟。Twofi 是 OSINT 工具之一。它由 DigiNinja 的 Robin Wood 编写，受知识共享署名-共享 2.0 许可协议保护。在本文中，将向您完整地介绍感兴趣的 Twitter 单词。此外，您将学习如何在 Kali Linux 上安装 Twofi。

要购买您自己的 [Linux VPS](https://eldernode.com/linux-vps/) ，请访问 [Eldernode](https://eldernode.com/) 上提供的软件包，感受其中的差异。

## **介绍并在 Kali Linux 上安装 two fi**

### **什么是 Twofi？**

Twofi 是一个工具，你可以用它来抓取用户或公司的 Twitter 信息。使用这些结果，您可以创建一个用于破解密码的自定义单词列表。当试图破解密码时，自定义单词列表是对标准目录非常有用的补充。第一次，人们在名为“高效黑客的 7 个习惯”的博客上读到了这个想法，这是关于使用 Twitter 来帮助根据搜索与被破解的列表相关的关键词来生成这些列表。因此，这个想法扩展到 Twofi，它将接受多个搜索项，并返回一个由最常见的第一个存储的单词列表。

### 两个 fi 版本

你需要一个 Twitter 账户和 Twitter API 密匙才能使用 Twofi。API 键需要输入到 Twofi 的配置文件中。

为了解释更多，让我们了解一下 Twofi 的版本，2.0-beta 版和 1.0 版

Twofi 有两个版本，你可以从它的官方网站下载。不同版本之间的区别在于版本 1 使用了现在已经删除的 Twitter 搜索特性(不需要认证)。

版本 2 使用了新的 API，它要求你有一个 Twitter 账户并应用 API 密匙。这样，你不需要现金，不需要等待人类的批准，所以这似乎是一个非常简单的过程。要使用该版本，您需要在浏览器中打开[apps.twitter.com](https://developer.twitter.com/en/apps)，并填写您的详细信息。正如我们提到的，所需的密钥对将会给你。将它们放入 *twofi.yml* 配置文件中。

由于脚本期望配置文件与 twofi 运行在同一个目录中，如果不是这样，您需要使用–config 参数来告诉配置文件在哪里。

### **如何在 Kali Linux 上安装 two fi**

使用下面的命令在 Kali 上安装 Twofi 并安装任何依赖的包

```
sudo apt-get install twofi
```

你可以在这里找到 Kali Linux 的 twofi 打包。

## **结论**

在本文中，我们介绍了如何在 Kali Linux 上安装 Twofi。你可以利用社交媒体平台来描述一个用户。Twofi 就是其中之一，它利用 Twitter API 生成一个定制的单词列表，可以用于离线密码破解。在 [Kali Linux 教程](https://blog.eldernode.com/tag/kali-linux/)上追踪更多相关文章，在 Eldernode 的[社区](https://community.eldernode.com/)上找到您可能的故障排除答案，或者成为可以帮助您的朋友解决问题的人。