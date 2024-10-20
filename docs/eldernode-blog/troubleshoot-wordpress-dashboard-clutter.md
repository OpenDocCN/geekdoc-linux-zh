# WordPress 仪表盘混乱故障排除- Eldernode

> 原文：<https://blog.eldernode.com/troubleshoot-wordpress-dashboard-clutter/>

![Troubleshoot WordPress Dashboard Clutter](img/ed3b3b976dcac832fd2efd2f55a9b11a.png)

WordPress 站点管理员可能会遇到的一个主要错误是 WordPress 仪表盘的混乱。在这篇文章中，我们将教你如何从 WordPress 教程系列中排除 WordPress dashboard 混乱，这样你就可以在这个问题发生时修复它。

WordPress 被认为是网络世界中流行且强大的内容管理工具。令人愉快和用户友好的 WordPress 环境是 WordPress 鼓励网站管理员使用 WordPress 内容管理的好处之一。但有时这个漂亮的仪表板可能会因为一些变化或可能出现的问题而出现问题。你也可以购买 [WordPress VPS 主机](https://eldernode.com/wordpress-vps/)。WordPress 虚拟服务器主机适用于那些网站流量很大，并且消耗不止一个共享主机资源的用户。

WordPress 仪表盘混乱是使用 WordPress 的网站管理员可能遇到的问题之一。在这个简短的[教程](https://eldernode.com/category/tutorial/)中，我们试图通过给 **WordPress php 文件**添加代码来解决 WordPress 仪表盘混乱的问题。为了解决 WordPress 仪表盘混乱的问题，你必须完成以下步骤。

## 解决 WordPress 仪表板混乱的问题

首先，通过你的面板进入**文件管理测试**。

**2-** 进入 **Public_html** 文件夹，进入 **wp-admin** 文件夹。

**3-** 在 wp-admin 文件夹下，下载 load-style.php 的**文件。**

**4-** 用**记事本++** 或**记事本**软件打开。

在页面顶部，找到短语**错误报告。**

用短语 **E_ALL | E_STRICT** 代替短语(0)。

**7-** 然后保存 load-style.php 的**文件并上传到你的主机，用**替换**的 load-style.php 文件。**

**8-** 完成以上步骤后，返回 **public_html** 文件夹。

**9-** 下载 wp-config.php 的**文件。**

**10-** 打开它，在文件末尾添加以下短语。

```
define( 'CONCATENATE_SCRIPTS', false ); 
```

**11-** 添加短语后，保存文件并上传到主机上，替换之前的文件。

完成这些步骤后，你将能够对你的 WordPress 仪表盘进行故障诊断。

***注意:*** 如果你的 WordPress 仪表盘杂乱的问题没有解决，你将需要手动安装一次 WordPress。

### 如何手动安装 WordPress

**1-** 下载[最新版本的 WordPress](https://wordpress.org/download/) 。

**2-** 上传到你的主机，解压 zip 模式。

已经创建了一个名为 wordpress 的文件夹，进入其中。

**4-** 选择 WordPress 文件夹中的整个文件，复制到 **public_html** 文件夹。

在这一部分，WordPress 将被手动重新安装到你的主机上，你的 WordPress 仪表盘混乱的问题将被解决。

## 结论

今天，WordPress 被成千上万的用户广泛使用，WordPress 仪表盘和桌面是内容管理系统中用户友好的环境之一，但有时这个漂亮的仪表盘由于一些变化或问题导致 WordPress 仪表盘崩溃。在本教程中，我们试图解决 WordPress 仪表盘混乱的问题。