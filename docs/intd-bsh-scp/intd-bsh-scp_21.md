# 使用 Bash 和 SSMTP 发送电子邮件

SSMTP 是一个将电子邮件从计算机或服务器发送到配置的邮件主机的工具。

SSMTP 本身不是一个电子邮件服务器，它不会接收电子邮件或管理队列。

它的主要用途之一是转发来自您的机器到外部电子邮件地址的自动化电子邮件（如系统警报）。

## 前提条件

为了成功完成本教程，您需要以下东西：

+   以非 root 用户身份访问 Ubuntu 18.04 服务器，具有 sudo 权限，并在您的服务器上安装了活动防火墙。有关设置这些信息，请参阅我们的[Ubuntu 18.04 初始服务器设置指南](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18.04)

+   一个 SMTP 服务器以及 SMTP 用户名和密码，这也可以与 Gmail 的 SMTP 服务器一起使用，或者您可以通过遵循本教程中的步骤来设置自己的 SMTP 服务器：[如何在 Ubuntu 16.04 上安装和配置 Postfix 作为仅发送的 SMTP 服务器](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-as-a-send-only-smtp-server-on-ubuntu-16.04)

## 安装 SSMTP

为了安装 SSMTP，您首先需要使用以下命令更新您的 apt 缓存：

```sh
      sudo apt update 

```

然后运行以下命令来安装 SSMTP：

```sh
      sudo apt install ssmtp 

```

您还需要安装的是`mailutils`，为此请运行以下命令：

```sh
      sudo apt install mailutils 

```

## 配置 SSMTP

现在您已经安装了`ssmtp`，为了在发送电子邮件时配置它使用您的 SMTP 服务器，您需要编辑 SSMTP 配置文件。

使用您喜欢的文本编辑器打开`/etc/ssmtp/ssmtp.conf`文件：

```sh
      sudo nano /etc/ssmtp/ssmtp.conf 

```

您需要包含您的 SMTP 配置：

```sh
      root=postmaster
mailhub=<^>your_smtp_host.com<^>:587
hostname=<^>your_hostname<^>
AuthUser=<^>your_gmail_username@your_smtp_host.com<^>
AuthPass=<^>your_gmail_password<^>
FromLineOverride=YES
UseSTARTTLS=YES 

```

保存文件并退出。

## 使用 SSMTP 发送电子邮件

完成配置后，为了发送电子邮件，只需运行以下命令：

```sh
      echo "<^>Here add your email body<^>" | mail -s "<^>Here specify your email subject<^>" <^>your_recepient_email@yourdomain.com<^> 

```

您可以直接在终端中运行此命令，或者将其包含在您的 bash 脚本中。

## 使用 SSMTP 发送文件（可选）

如果您需要发送文件作为附件，可以使用`mpack`。

要安装`mpack`，请运行以下命令：

```sh
      sudo apt install mpack 

```

接下来，为了发送带有附件的电子邮件，请运行以下命令。

```sh
      mpack -s "<^>Your Subject here<^>" your_file.zip <^>your_recepient_email@yourdomain.com<^> 

```

上述命令将发送一封带有`<^>your_file.zip<^>`附件的电子邮件到`<^>your_recepient_email@yourdomain.com<^>`。

## 结论

SSMTP 是在 bash 脚本中直接实现 SMTP 电子邮件功能的一个很好的可靠方法。

关于 SSMTP 的更多信息，我建议您查看官方文档[这里](https://wiki.archlinux.org/index.php/SSMTP)。

> **注意：**此内容最初发布在[DigitalOcean 社区论坛](https://www.digitalocean.com/community/questions/how-to-send-emails-from-a-bash-script-using-ssmtp)。
