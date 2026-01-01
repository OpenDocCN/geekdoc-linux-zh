# 启用 TLS

curl 支持许多协议的 TLS 版本。HTTP 有 HTTPS，FTP 有 FTPS，LDAP 有 LDAPS，POP3 有 POP3S，IMAP 有 IMAPS，SMTP 有 SMTPS。

如果服务器端支持，您可以使用 curl 的这些协议的 TLS 版本。

在协议中使用 TLS 有两种一般方法。其中一种是在第一次连接握手时就使用 TLS，另一种是使用特定协议的指令将连接从纯文本升级到 TLS。

使用 curl，如果您在 URL 中明确指定了协议的 TLS 版本（以 'S' 字符结尾的名称），curl 会尝试从开始就使用 TLS 连接，而如果您在 URL 中指定了非 TLS 版本，您通常可以使用 `--ssl` 选项将连接升级到基于 TLS 的连接。

支持表看起来如下：

| 清除 | TLS 版本 | –ssl |
| --- | --- | --- |
| HTTP | HTTPS | no |
| LDAP | LDAPS | no |
| FTP | FTPS | **yes** |
| POP3 | POP3S | **yes** |
| IMAP | IMAPS | **yes** |
| SMTP | SMTPS | **yes** |

所有能够执行 `--ssl` 的协议都倾向于这种方法。使用 `--ssl` 意味着 curl 会尝试将连接升级到 TLS，但如果失败，它仍然会使用协议的纯文本版本继续传输。为了使 `--ssl` 选项**要求**TLS 继续传输，可以使用 `--ssl-reqd` 选项，如果 curl 无法成功协商 TLS，则传输会失败。

为您的 FTP 传输要求 TLS 安全性：

```sh
curl --ssl-reqd ftp://ftp.example.com/file.txt
```

建议使用 TLS 进行您的 FTP 传输：

```sh
curl --ssl ftp://ftp.example.com/file.txt
```

直接使用 TLS 连接（到 `HTTPS://`，`LDAPS://`，`FTPS://` 等）意味着 TLS 是强制性的，如果未协商 TLS，curl 会返回错误。

通过 HTTPS 获取文件：

```sh
curl https://www.example.com/
```
