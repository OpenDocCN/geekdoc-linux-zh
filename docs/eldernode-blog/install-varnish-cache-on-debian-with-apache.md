# 教程用 Apache - Eldernode 博客在 Debian 上安装 Varnish 缓存

> 原文：<https://blog.eldernode.com/install-varnish-cache-on-debian-with-apache/>

![Tutorial Install Varnish Cache on Debian with Apache](img/8db5d75a65a3aa44b3b11537894574e5.png)

Varnish Cache 是开源的，因此，该软件属于自由软件许可家族，允许以最小的限制使用和分发。在本文中，我们将教你如何用 Apache 在 Debian 上安装 Varnish Cache。如果你想购买一台 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务器，你可以访问 [Eldernode](https://eldernode.com/) 中的软件包。

## **如何在 Debian Linux 上安装 Varnish Cache**

### 清漆缓存功能

Varnish 在用户第一次访问 web 服务器提供的页面时获取并保存该页面的副本。这将加快网站的速度。下次用户请求同一个页面时，Varnish 会提供一个副本，而不是从 web 服务器请求页面。这样，您的网络服务器将管理更少的流量，您的网站性能和可扩展性将更高。根据您的架构，Varnish cache 可将您的 web 内容交付提高 80%或更多。

## **用 Apache** 在 Debian 上安装 Varnish Cache

### 步骤 1) **安装 Apache**

要安装和配置 Varnish 缓存，我们必须首先在我们的节点上安装 Apache。这个过程的第一步是更新 debian 操作系统:

```
apt update
```

```
apt upgrade
```

下载软件包后，更新操作系统并安装 Apache 2.4:

```
apt -y install apache2
```

安装完成后，启用 Apache 服务:

```
systemctl enable apache2 && systemctl start apache2
```

如果启用 UFW，可以使用以下命令。如果 UFW 未启用，请跳过以下命令:

```
ufw allow 80/tcp
```

```
ufw allow 443/tcp
```

```
ufw reload
```

### 步骤 2) **安装并配置清漆缓存**

完成 Apache 安装后，就该安装 Varnish 缓存了。
第一步是下载 GPG 密钥，然后添加它:

```
curl -L https://packagecloud.io/varnishcache/varnish5/gpgkey | sudo apt-key add -
```

接下来，必须安装软件包要求:

```
apt install -y debian-archive-keyring apt-transport-https
```

然后必须将存储库添加到源列表中:

```
echo "deb https://packagecloud.io/varnishcache/varnish5/debian/ stretch main" >
```

```
/etc/apt/sources.list.d/varnishcache5.list
```

还需要更新包管理器来添加新的存储库:

```
apt update
```

既然已经安装了存储库，并且更新了必要的组件，那么可能会安装 Varnish Cache:

```
apt install -y varnish
```

在每次安装结束时，检查安装的版本是否与您期望的相匹配是很重要的。有必要检查 Varnish 缓存的安装及其适当的版本:

```
varnishd -V
```

确认安装后，通过编辑以下文件将 Varnish 缓存配置为监听端口 80，并将端口 80 替换为端口 6081:

```
nano /lib/systemd/system/varnish.service
```

出发地:

```
ExecStart=/usr/sbin/varnishd -a :6081 -T localhost:6082 -f /etc/varnish/default.vcl -S /etc/varnish/secret -s malloc,256m
```

收件人:

```
ExecStart=/usr/sbin/varnishd -a :80 -T localhost:6082 -f /etc/varnish/default.vcl -S /etc/varnish/secret -s malloc,256m
```

编辑文件后，我们必须重新加载系统守护程序来更新文件:

```
systemctl daemon-reload
```

重新加载后，应修改默认配置，并告知系统将收到的 Varnish 缓存请求定向到哪里:

```
nano /etc/varnish/default.vcl  backend default {  .host = "127.0.0.1";  .port = "8080";  }
```

Apache 2.4 也需要一些小的改动，允许它监听端口 8080，而不是端口 80:

```
nano /etc/apache2/ports.conf
```

```
Listen 8080
```

```
nano /etc/apache2/sites-enabled/000-default.conf  <VirtualHost *:8080>
```

修复之后，是时候重启 Apache 2.4 和 Varnish 缓存了。完全重启后，启用清漆缓存:

```
systemctl restart apache2
```

```
systemctl restart varnish
```

```
systemctl enable varnish
```

在这一点上，我们测试 Varnish Cache 和 Apache 2.4，以确保它们都能按预期工作:

```
curl -I http://127.0.0.1  HTTP/1.1 200 OK  Server: Apache/2.4.25 (Debian)  Vary: Accept-Encoding  Content-Type: text/html  X-Varnish: 2  Age: 0  Via: 1.1 varnish (Varnish/5.1)  EW/"29cd-55771150d08a7-gzip"  Accept-Ranges: bytes  Connection: keep-alive
```

如有必要，您可以使用以下命令检查 Varnish 缓存统计信息:

```
varnishstat
```

## 结论

那么，您已经成功地在您的 Debian Linux 上安装并配置了 Apache 2.4 和 Varnish Cache。此安装允许您简化网页的分发，并允许用户比以前更快、更有效地访问信息。