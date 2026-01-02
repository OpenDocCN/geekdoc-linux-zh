# 使用 Bash 与 Cloudflare API 一起工作

我所有的网站都托管在 **DigitalOcean** Droplets 上，我还使用 Cloudflare 作为我的 CDN 提供商。使用 Cloudflare 的一个好处是它减少了用户整体流量，同时也隐藏了您实际服务器的 IP 地址在其 CDN 后面。

我个人最喜欢的 Cloudflare 功能是他们的免费 DDoS 保护。它已经多次从不同的 DDoS 攻击中拯救了我的服务器。他们有一个很酷的 API，您可以使用它轻松地启用和禁用他们的 DDoS 保护。

本章将是一个练习！我挑战您编写一个简短的 bash 脚本，该脚本可以在需要时自动启用和禁用服务器的 Cloudflare DDoS 保护！

## 前提条件

在遵循此指南之前，请设置您的 Cloudflare 账户并准备好您的网站。如果您不确定如何操作，可以按照以下步骤进行：[创建 Cloudflare 账户并添加网站](https://support.cloudflare.com/hc/en-us/articles/201720164-Step-2-Create-a-Cloudflare-account-and-add-a-website)。

一旦您拥有 Cloudflare 账户，请确保获取以下信息：

+   一个 Cloudflare 账户

+   Cloudflare API 密钥

+   Cloudflare 区域 ID

此外，确保您的服务器上已安装 curl：

```sh
      curl --version 

```

如果 curl 未安装，您需要运行以下命令：

+   对于 RedHat/CentOs：

```sh
      yum install curl 

```

+   对于 Debian/Ubuntu：

```sh
      apt-get install curl 

```

## 挑战 - 脚本要求

脚本需要监控服务器的 CPU 使用情况，如果根据 vCPU 的数量 CPU 使用率变高，它将通过 Cloudflare API 自动启用 Cloudflare DDoS 保护。

脚本的主要功能应包括：

+   检查服务器上的脚本 CPU 负载

+   在 CPU 峰值的情况下，脚本会触发对 Cloudflare 的 API 调用，并为指定的区域启用 DDoS 保护功能

+   当 CPU 负载恢复正常时，脚本将禁用“我正在遭受攻击”选项，并将其恢复到正常状态

## 示例脚本

我已经准备了一个演示脚本，您可以将其作为参考。但我鼓励您先尝试自己编写脚本，然后再查看我的脚本！

要下载脚本，只需运行以下命令：

```sh
      wget https://raw.githubusercontent.com/bobbyiliev/cloudflare-ddos-protection/main/protection.sh 

```

使用您最喜欢的文本编辑器打开脚本：

```sh
      nano protection.sh 

```

并使用您的 Cloudflare 详细信息更新以下信息：

```sh
      CF_CONE_ID=YOUR_CF_ZONE_ID
CF_EMAIL_ADDRESS=YOUR_CF_EMAIL_ADDRESS
CF_API_KEY=YOUR_CF_API_KEY 

```

然后，使脚本可执行：

```sh
      chmod +x ~/protection.sh 

```

最后，设置两个每 30 秒运行一次的 Cron 任务。要编辑您的 crontab，请运行：

```sh
      crontab -e 

```

并添加以下内容：

```sh
      * * * * * /path-to-the-script/cloudflare/protection.sh
* * * * * ( sleep 30 ; /path-to-the-script/cloudflare/protection.sh ) 

```

注意，您需要将脚本的路径更改为实际存储脚本的路径。

## 结论

这是一个相当直接且经济的解决方案，脚本的一个缺点是，如果您的服务器因攻击而变得无响应，脚本可能根本不会被触发。

当然，更好的方法是使用像 Nagios 这样的监控系统，然后根据监控系统的统计数据来触发脚本，但这个脚本挑战可以是一个很好的学习经验！

这里是另一个关于如何使用 Discord API 以及如何通过 Bash 脚本向您的 Discord 频道发送通知的优质资源：

[如何在 Ubuntu 18.04 上使用 Discord Webhooks 获取网站状态通知](https://www.digitalocean.com/community/tutorials/how-to-use-discord-webhooks-to-get-notifications-for-your-website-status-on-ubuntu-18-04)

> **注意：** 此内容最初发布在 [DevDojo](https://devdojo.com/bobbyiliev/bash-script-to-automatically-enable-cloudflare-ddos-protection)
