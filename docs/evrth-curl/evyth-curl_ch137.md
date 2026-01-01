# 发送电子邮件

使用 curl 通过 SMTP 协议发送电子邮件。SMTP 代表[简单邮件传输协议](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)。

curl 支持向 SMTP 服务器发送数据，结合正确的命令行选项，可以将电子邮件发送到你选择的接收者。

当使用 curl 发送 SMTP 时，有两个必要的命令行选项**必须**使用。

+   你需要使用`--mail-rcpt`告诉服务器至少一个收件人。你可以多次使用此选项，然后 curl 会告诉服务器所有这些电子邮件地址都应该收到邮件。

+   你需要使用`--mail-from`告诉服务器哪个电子邮件地址是邮件的发送者。重要的是要意识到，这个电子邮件地址不一定与电子邮件文本中显示的`From:`行相同。

然后，你需要提供实际的电子邮件数据。这是一个根据[RFC 5322](https://tools.ietf.org/html/rfc5322.html)格式化的（文本）文件。它是一组头信息和正文。头信息和正文都需要正确编码。头信息通常包括`To:`, `From:`, `Subject:`, `Date:`等。

发送电子邮件的基本命令：

```sh
curl smtp://mail.example.com --mail-from myself@example.com --mail-rcpt \
receiver@example.com --upload-file email.txt
```

一个`email.txt`的例子可能如下所示：

```sh
From: John Smith <john@example.com>
To: Joe Smith <smith@example.com>
Subject: an example.com example email
Date: Mon, 7 Nov 2016 08:45:16

Dear Joe,
Welcome to this example email. What a lovely day.
```

## 安全邮件传输

一些邮件提供商允许或要求使用 SSL 进行 SMTP。它们可能使用一个专用的 SSL 端口，或者允许在明文连接上升级 SSL。

如果你的邮件提供商有一个专用的 SSL 端口，你可以使用 smtps://代替 smtp://，它默认使用 SMTP SSL 端口 465，并要求整个连接都是 SSL。例如 smtps://smtp.gmail.com/。

然而，如果你的提供商允许从明文传输升级到安全传输，你可以使用以下选项之一：

```sh
--ssl           Try SSL/TLS (FTP, IMAP, POP3, SMTP)
--ssl-reqd      Require SSL/TLS (FTP, IMAP, POP3, SMTP)
```

你可以通过在命令中添加`--ssl`来告诉 curl*尝试*但不要求升级到安全传输：

```sh
curl --ssl smtp://mail.example.com --mail-from myself@example.com \
     --mail-rcpt receiver@example.com --upload-file email.txt \
     --user 'user@your-account.com:your-account-password'
```

你可以通过在命令中添加`--ssl-reqd`来告诉 curl*要求*升级到使用安全传输：

```sh
curl --ssl-reqd smtp://mail.example.com --mail-from myself@example.com \
     --mail-rcpt receiver@example.com --upload-file email.txt \
     --user 'user@your-account.com:your-account-password'
```

## SMTP URL

SMTP 请求的路径部分指定了在与邮件服务器通信时呈现的主机名。如果省略路径，curl 将尝试确定本地计算机的主机名并使用它。然而，这可能不会返回某些邮件服务器所需的完全限定域名，指定此路径允许你设置一个替代名称，例如从外部函数（如 gethostname 或 getaddrinfo）获得的计算机的完全限定域名。

要连接到`mail.example.com`上的邮件服务器并将本地计算机的主机名发送到`HELO`或`EHLO`命令中：

```sh
curl smtp://mail.example.com
```

你当然可以使用`-v`选项来查看客户端与服务器之间的通信。

要让 curl 在`HELO`/`EHLO`命令中将`client.example.com`发送到`mail.example.com`上的邮件服务器，请使用：

```sh
curl smtp://mail.example.com/client.example.com
```

## 无 MX 查找！

当你使用普通邮件客户端发送电子邮件时，它首先会检查你想要发送电子邮件的特定域的 MX 记录。如果你向`joe@example.com`发送电子邮件，客户端会获取`example.com`的 MX 记录，以确定在向 example.com 用户发送电子邮件时应使用哪个邮件服务器（或服务器组）。

`curl`本身不会执行 MX 查找。如果你想确定向特定域发送电子邮件时应使用哪个服务器，我们建议你首先确定这一点，然后调用`curl`来使用这些服务器。用于获取 MX 记录的有用命令行工具包括‘dig’和‘nslookup’。
