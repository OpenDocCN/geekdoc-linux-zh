# 身份验证

每个 HTTP 请求都可以进行身份验证。如果服务器或代理希望用户提供证明他们拥有访问 URL 或执行操作的正确凭据，它可以发送一个 HTTP 响应代码，告知客户端它需要提供一个正确的 HTTP 身份验证报头才能允许请求。

需要身份验证的服务器会返回一个 401 响应代码，并附带一个 `WWW-Authenticate:` 报头，其中列出了服务器支持的所有身份验证方法。

需要身份验证的 HTTP 代理会返回一个 407 响应代码，并附带一个 `Proxy-Authenticate:` 报头，其中列出了代理支持的所有身份验证方法。

值得注意的是，如今的大多数网站在登录等操作中不需要 HTTP 身份验证，而是要求用户在网页上登录，然后浏览器发出包含用户名和密码等的 POST 请求，并随后维护会话的 cookies。

要让 curl 执行身份验证的 HTTP 请求，您可以使用 `-u, --user` 选项提供用户名和密码（用冒号分隔）。例如：

```sh
curl --user daniel:secret http://example.com/
```

这使得 curl 使用默认的 *基本* HTTP 身份验证方法。是的，它实际上被称为基本，并且确实是基本的。要显式请求基本方法，请使用 `--basic`。

基本身份验证方法会将用户名和密码以明文形式（base64 编码）通过网络发送，应避免用于 HTTP 传输。

当请求使用单一（指定或隐含）的身份验证方法进行 HTTP 传输时，curl 会将身份验证报头插入到第一个网络请求中。

如果您希望 curl 首先测试身份验证是否真的需要，您可以请求 curl 查找并自动使用它所知道的最高安全方法，使用 `--anyauth`。这使得 curl 尝试未认证的请求，并在必要时切换到身份验证：

```sh
curl --anyauth --user daniel:secret http://example.com/
```

同样的概念也适用于可能需要身份验证的 HTTP 操作：

```sh
curl --proxy-anyauth --proxy-user daniel:secret http://example.com/ \
     --proxy http://proxy.example.com:80/
```

curl 通常（根据其构建方式略有不同）还支持几种其他身份验证方法，包括摘要（Digest）、协商（Negotiate）和 NTLM。您也可以明确请求这些方法：

```sh
curl --digest --user daniel:secret http://example.com/
curl --negotiate --user daniel:secret http://example.com/
curl --ntlm --user daniel:secret http://example.com/
```
