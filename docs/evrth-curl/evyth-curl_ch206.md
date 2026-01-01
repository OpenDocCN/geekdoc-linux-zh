# TLS 选项

在撰写本文时，curl_easy_setopt 至少有**四十**个不同的选项是专门用于控制 libcurl 如何进行 SSL 和 TLS。

使用 TLS 进行的传输使用安全默认值，但由于 curl 被用于许多不同的场景和配置中，您可能会遇到想要更改这些行为的情况。

## 协议版本

使用`CURLOPT_SSLVERSION`和`CURLOPT_PROXY_SSLVERSION`，您可以指定您可接受的 SSL 或 TLS 协议范围。传统上，SSL 和 TLS 协议版本随着时间的推移被发现检测不合适，即使 curl 本身随着时间的推移提高了默认的较低版本，您可能只想使用最新和最安全的协议版本。

这些选项接受最低可接受版本和可选的最高版本。如果服务器无法根据这些条件协商连接，传输将失败。

示例：

```sh
curl_easy_setopt(easy, CURLOPT_SSLVERSION, CURL_SSLVERSION_TLSv1_2);
```

## 协议细节和行为

您可以通过设置`CURLOPT_SSL_CIPHER_LIST`和`CURLOPT_PROXY_SSL_CIPHER_LIST`来选择要使用的加密套件。

您可以使用`CURLOPT_SSL_FALSESTART`请求启用 SSL“False Start”，并且还有其他一些行为变化可以通过`CURLOPT_SSL_OPTIONS`进行调整。

## 验证

使用 TLS 的客户端需要验证它与服务器通信的是正确且受信任的。这是通过验证服务器的证书是否由 curl 具有公钥的证书机构（CA）签名，并且证书包含服务器的名称来完成的。任何这些检查失败都会导致传输失败。

为了开发目的和实验，curl 允许应用程序关闭服务器或 HTTPS 代理的任一或两个检查。

+   `CURLOPT_SSL_VERIFYPEER`控制检查证书是否由受信任的 CA 签名。

+   `CURLOPT_SSL_VERIFYHOST`控制对证书中名称的检查。

+   `CURLOPT_PROXY_SSL_VERIFYPEER`是`CURLOPT_SSL_VERIFYPEER`的代理版本。

+   `CURLOPT_PROXY_SSL_VERIFYHOST`是`CURLOPT_SSL_VERIFYHOST`的代理版本。

可选地，您可以通过使用`CURLOPT_PINNEDPUBLICKEY`或`CURLOPT_PROXY_PINNEDPUBLICKEY`告诉 curl 验证证书的公钥与已知散列相匹配。在这里，不匹配也会导致传输失败。

## 认证

### TLS 客户端证书

当使用 TLS 并且服务器要求客户端使用证书进行认证时，您通常使用`CURLOPT_SSLKEY`和`CURLOPT_SSLCERT`指定私钥和相应的客户端证书。通常还需要设置密钥的密码，使用`CURLOPT_SSLKEYPASSWD`。

再次强调，对于连接到 HTTPS 代理的选项，存在一组独立的选项：`CURLOPT_PROXY_SSLKEY`、`CURLOPT_PROXY_SSLCERT`等。

### TLS 认证

TLS 连接提供了一个（很少使用）称为安全远程密码的功能。使用此功能，您可以使用名称和密码对服务器连接进行认证，选项称为`CURLOPT_TLSAUTH_USERNAME`和`CURLOPT_TLSAUTH_PASSWORD`。

## STARTTLS

对于使用 STARTTLS 方法升级连接到 TLS 的协议（如 FTP、IMAP、POP3 和 SMTP），你通常在指定 URL 时告诉 curl 使用该协议的非 TLS 版本，然后请求 curl 使用`CURLOPT_USE_SSL`选项启用 TLS。

此选项允许客户端在无法升级到 TLS 时让 curl 继续执行，但这并不是推荐的做法，因为这样你可能会在不经意间使用一个不安全的协议。

```sh
/* require use of SSL for this, or fail */
curl_easy_setopt(curl, CURLOPT_USE_SSL, CURLUSESSL_ALL);
```
