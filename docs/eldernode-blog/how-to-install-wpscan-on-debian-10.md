# 如何在 Debian 10[安全] - Eldernode 上安装 wpscan

> 原文：<https://blog.eldernode.com/how-to-install-wpscan-on-debian-10/>

![How to install wpscan on Debian 10 [Security]](img/996ebf2b4dcc46b8f0602d49239ca1e3.png)

教程如何**在 Debian 10** Linux VPS 服务器上安装 wpscan。WPScan 是一款流行的扫描工具[。网站安全是站长们一直关注的最重要的问题之一。目前有超过 6000 万的网站在使用 WordPress。发现 WordPress 站点漏洞及其安全问题的一种方法是使用扫描仪。这些相对流行的扫描仪之一是 WPScan。有了](https://blog.eldernode.com/tag/wordpress/) [WPScan](https://wpscan.org/) 你可以扫描你的 WordPress 网站，获得诸如网站上安装的插件和模板的漏洞、有安全漏洞的 WordPress 版本以及其他安全项目等信息。如果你需要[购买 WordPress 主机 VPS](https://eldernode.com/wordpress-vps/) ，你可以在 [Eldernode](https://eldernode.com/) 中看到可用的包。

**如何在 Debian 10 服务器上安装和使用 wps can**

WordPress 是一个内容管理系统或 CMS，被世界上大部分网站使用。还有一点可以提的是，WordPress 有非常强大的支持，而且是可以开发的开源。到目前为止，已经开发了许多插件来开发它，因此它得到了广泛的发展。随着网站的成长，控制安全以防止被黑客攻击肯定会变得更加困难。

wpscan 是 WordPress 内容管理系统最流行的安全扫描工具之一，比其他 WordPress 扫描器拥有更多的功能和普及性。这就是为什么这个工具在 Kali Linux 等各种黑客操作系统上默认可用的原因。但是如果你需要把它安装在 Debian 等其他 Linux 操作系统上，你可以这样做，用它来进行全面扫描和渗透测试，寻找 WordPress 网站的漏洞。

在本教程中，我们将介绍 WordPress 最好的漏洞扫描工具之一，即 wpscan，在接下来的内容中，我们将教你如何在 Debian 10 中安装 wpscan。

## 教程在 Linux Debian 10 上逐步配置和使用 wpscan

在第一步中，为 WPScan 安装 WPScan 依赖项:

```
sudo apt-get update 
```

```
sudo apt-get install curl git libcurl4-openssl-dev make zlib1g-dev gawk g++ gcc libreadline6-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 autoconf libgdbm-dev libncurses5-dev automake libtool bison pkg-config -y
```