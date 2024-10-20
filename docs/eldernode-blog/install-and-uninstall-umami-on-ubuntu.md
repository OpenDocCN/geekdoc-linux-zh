# 如何在 Ubuntu 20.04 - Eldernode 博客上安装和卸载 Umami

> 原文：<https://blog.eldernode.com/install-and-uninstall-umami-on-ubuntu/>

![How to Install and Uninstall Umami on Ubuntu 20.04](img/a54c561c3a3a6fa39cb304c7ba4ca43a.png)

Umami 是一个开源的网络分析程序。它注重简单，设计精良，快捷，注重隐私。Umami 是用 Node.js 编写的，是一个自托管的网络分析工具，可以替代专注于隐私的 Google Analytics。Umami 可以在 MySQL 或 PostgreSQL 数据库中存储关于网站访问者的数据。在这篇文章中，我们将教你如何在 Ubuntu 20.04 上安装和卸载 Umami。如果你想购买一台 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务器，你可以访问 [Eldernode](https://eldernode.com/) 中的软件包。

## 教程在 Ubuntu Linux 上安装和卸载 Umami

### 在 Ubuntu 20.04 | Ubuntu 21.04 上安装 Umami

该存储库将下载到/opt 文件夹中。

```
cd /opt
```

现在，您应该通过输入以下命令从 GitHub 克隆存储库:

```
sudo git clone https://github.com/mikecao/umami.git
```

所有软件和配置文件都将位于/opt/umami 中。然后转到新创建的鲜味目录:

```
cd umami
```

在这一步，您需要更新项目 docker-compose.yml 文件。这个文件是它用来同时配置和启动多个 Docker 容器的。您必须更改该文件中的两个选项:Umami 绑定到的 IP，以及在加密数据库中的内容时用作 salt 的随机散列。

下一步，生成一个新的随机散列粘贴到文件中。为此，请输入以下命令:

```
openssl rand -base64 32
```

`openssl 命令用于生成 32 个随机字符。您应该首先将输出复制到剪贴板，然后使用以下命令打开配置文件:`

```
`sudo nano docker-compose.yml`
```

`现在您应该找到 HASH_SALT 选项并删除占位符文本。然后记住通过输入以下命令粘贴随机散列:`

```
`HASH_SALT: replace-me-with-a-random-string`
```

`然后您应该找到端口:`

```
`ports: - "127.0.01:3000:3000"`
```

`要更新值“3000:3000 ”,只需在其上添加 127.0.0.1:。这确保了 Umami 不是公开可用的，并且只在本地主机接口上侦听。`

`即使设置了 UFW 防火墙，由于 Docker 网络工作方式的一些奇怪之处，如果您不执行此步骤，您的鲜味容器将在端口 3000 上对公众可用。`

`现在你应该通过按 CTRL+O 保存文件，然后在 nano 中输入。最后，按 CTRL+X 关闭编辑器。`

`在这个阶段，您需要设置两个容器。为此，您可以使用以下命令:`

```
`sudo docker-compose up --detach`
```

`Docker-compose 在后台创建容器，通过声明–detach 标志从终端会话中分离出来:`

```
`Creating umami_db_1 ... done`
```

```
`Creating umami_umami_1 ... done`
```

`您可以使用以下命令验证获取在本地主机上运行的新 Umami 容器的主页:`

```
`curl localhost:3000`
```

`输出:`

```
`<!DOCTYPE html><html><head><meta charSet="utf-8"/> . . .`
```

`如果一大块 HTML 输出到您的终端，这表明 Umami 服务器已经启动并正在运行。`

### `**在 Ubuntu Linux 上配置 Nginx**`

`首先，使用以下命令更新您的包列表:`

```
`sudo apt update`
```

`现在，您可以通过输入以下命令来安装 Nginx:`

```
`sudo apt install nginx`
```

`接下来，您需要允许端口 80 和 443 进行通信。为此，请使用“Nginx Full”UFW 应用配置文件:`

```
`sudo ufw allow "Nginx Full"`
```

`输出:`

```
`Rule added`
```

```
`Rule added (v6)`
```

`现在，您应该在/etc/nginx/sites-available 目录中打开一个新的 [Nginx](https://blog.eldernode.com/secure-nginx-encrypt-debian-10/) 配置文件:`

```
`sudo nano /etc/nginx/sites-available/umami.conf`
```

`然后将以下内容粘贴到新的配置文件中。记得用您配置的指向 Umami 服务器的域替换您的 _domain_here。`

```
`server {      listen        80;      listen        [::]:80;      server_name  your_domain_here;        access_log  /var/log/nginx/umami.access.log;      error_log   /var/log/nginx/umami.error.log;        location / {        proxy_pass http://localhost:3000;        proxy_set_header X-Real-IP $remote_addr;        proxy_set_header Host $host;        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;    }  }`
```

`上面的配置仅适用于 HTTP，因为您允许 Certbot 在下一步中配置 SSL。设置日志位置的配置，然后将所有流量传递到 http://localhost:3000。`

`现在您应该保存文件并关闭它。`

`然后您需要将配置链接到/etc/nginx/sites-enabled/来启用它。为此，请输入以下命令:`

```
 `sudo In -s /etc/nginx/sites-available/umami.conf /etc/nginx/sites-enabled/`
```

`在此步骤中，通过执行以下命令来验证配置文件的语法是否正确:`

```
`sudo nginx -t`
```

`输出:`

```
`nginx: the configuration file /etc/nginx/nginx.conf syntax is ok`
```

```
`nginx: configuration file /etc/nginx/nginx.conf test is successful`
```

`最后，您必须重新加载 nginx 服务来获取新的配置。为此，请运行以下命令:`

```
`sudo systemctl reload nginx`
```

`鲜味网站可通过简单的 HTTP。如果加载 http://your_domain_here。`

### `**如何安装 Certbot 并设置 SSL 证书**`

`首先，您应该通过输入以下命令来安装 Certbot 及其 Nginx 插件:`

```
`sudo apt install certbot python3-certbot-nginx`
```

`您应该在–Nginx 模式下运行 certbot，并指定在 Nginx server_name 配置中使用的相同域。为此，请使用以下命令:`

```
`sudo certbot --nginx -d your_domain_here`
```

`现在，您应该同意“让我们加密服务条款”并输入您的电子邮件地址。`

`然后，您可以将所有 HTTP 流量重定向到 HTTPS，这样做是安全的。`

`接下来，让我们加密确认您的请求，Certbot 将下载您的证书。`

`您应该在以下位置测试您的配置:`

```
`https://www.ssllabs.com/ssltest/analyze.html?d=umami.example.com`
```

`Certbot 会自动重新加载 Nginx，以获取新的配置和认证。现在重新加载你的网站，如果你选择重定向选项，它会自动切换到 HTTPS。`

## `**如何卸载 Ubuntu 20.04 上的鲜味| Ubuntu 21.04**`

`通过输入以下命令，鲜味将从您的系统中删除。`

```
`sudo apt-get remove umami`
```

## `结论`

`在本文中，您了解了如何在 Ubuntu 20.04 上安装和卸载 umami。事实上，Umami 应用程序和 PostgreSQL 数据库是使用 Docker Compose 启动的，然后启动了 Nginx 反向代理。最后，您可以使用加密 SSL 证书来保护它。`