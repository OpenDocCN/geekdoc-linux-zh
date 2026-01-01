# 阅读邮件

在互联网上，用于从服务器读取/下载邮件（至少如果我们不计入基于 Web 的阅读）有两种主要的协议，它们是 IMAP 和 POP3。前者是稍微更现代的替代品。curl 支持这两种协议。

## POP3

列出消息编号和大小：

```sh
curl pop3://mail.example.com/
```

下载第 1 条消息：

```sh
curl pop3://mail.example.com/1
```

删除第 1 条消息：

```sh
curl --request DELE pop3://mail.example.com/1
```

## IMAP

使用 UID 57 从邮箱“东西”中获取邮件：

```sh
curl imap://server.example.com/stuff;UID=57
```

相反，从邮箱“有趣”中获取索引为 57 的邮件：

```sh
curl imap://server.example.com/fun;MAILINDEX=57
```

列出邮箱“无聊”中的邮件：

```sh
curl imap://server.example.com/boring
```

列出邮箱“无聊”中的邮件并输入用户名和密码：

```sh
curl imap://server.example.com/boring -u user:password
```

## 邮件传输层安全性（TLS）

POP3 和 IMAP 都可以通过安全连接进行，并且都可以使用显式或隐式 TLS。其中，“显式”方法可能是最常见的方法，这意味着客户端使用不安全的连接连接到服务器，并在过程中将其升级为 TLS，使用`STARTTLS`命令。你可以通过`--ssl`告诉 curl 尝试这样做，或者如果你想*坚持*使用安全连接，你可以使用`--ssl-reqd`。如下所示：

```sh
curl pop3://mail.example.com/ --ssl-reqd
```

或者

```sh
curl --ssl imap://mail.example.com/inbox
```

“隐式”SSL 意味着连接在第一次连接时就得到了加密，你可以通过在 URL 中指定使用 SSL 的方案来让 curl 尝试这样做。在这种情况下，可以是`pop3s://`或`imaps://`。对于此类连接，curl 坚持从开始就连接并协商 TLS 连接，否则它将失败操作。

之前使用隐式 SSL 的显式示例：

```sh
curl pop3s://mail.example.com/
```

或者

```sh
curl imaps://mail.example.com/inbox
```
