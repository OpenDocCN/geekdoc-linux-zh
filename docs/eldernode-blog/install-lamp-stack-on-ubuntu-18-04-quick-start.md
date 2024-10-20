# 在 Ubuntu 上安装灯组 18.04[快速启动]灯

> 原文：<https://blog.eldernode.com/install-lamp-stack-on-ubuntu-18-04-quick-start/>

![Install LAMP stack on Ubuntu 18.04 [quick-start]](img/2f3c17eb40c6a70c34d000d1d3526cbc.png)

在本教程中，我们讨论**通过 tasksel 在 Ubuntu 18.04 上安装 LAMP stack。**

不过我们先说说 [**灯堆**](https://eldernode.com/tag/lamp-stack/) 又是什么呢？

#### 什么是灯栈？

基于 Linux 的 web 服务器由四个软件组件组成。这些组件排列在相互支持的层中，构成了软件栈。网站和 Web 应用程序运行在这个底层堆栈之上。构成传统灯组的常见软件组件有:

**Linux:**Ubuntu、CentOS、Debian 等操作系统。构成了我们的第一层。Linux 为栈模型奠定了基础。

**Apache:** 第二层由 web 服务器软件组成，典型的有 [Apache Web 服务器](https://www.apache.org/)。这一层位于 Linux 层之上。Web 服务器负责从 web 浏览器翻译到正确的网站。

**MySQL:** 我们的第三层是数据库所在的地方。数据库引擎(MySQL)存储可以通过脚本查询以构建网站的详细信息。MySQL 通常与 Apache 一起位于 Linux 层之上。在高端配置中。

PHP: 坐在它们上面的是我们的第四层，也是最后一层。脚本层由 PHP 组成。网站和网络应用运行在这一层。

太好了。现在你知道什么是灯栈，阅读更多关于安装灯栈的内容。

**点:** 在本教程中你需要 root 权限，如果你用其他用户名有 sudo 权限，请在任何命令前加 sudo。

### 在 Ubuntu 18.04 上安装灯组

用 Tasksel 在 Ubuntu 18 上安装灯组

当你使用 tasksel 时，tasksel 只需一个命令就可以在 Ubuntu 上安装所有的服务(Apache，MySQL，PHP)。

默认情况下，如果尚未安装，请安装 Tasksel:

```
apt install tasksel
```

2-安装 tasksel 后，现在使用 Tasksel 安装灯组

```
tasksel install lamp-server
```

搞定了。2 或 3 分钟后(取决于你的 [VPS](https://eldernode.com/vps/) 或[专用](https://eldernode.com/))灯栈安装在你的 [Ubuntu 服务器](https://eldernode.com/ubuntu-vps/) 18.04 上

在本教程中，我们安装灯作为一个快速的开始，在未来，我们将手动安装灯栈。