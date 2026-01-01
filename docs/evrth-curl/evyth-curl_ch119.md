# 代理认证

HTTP 和 SOCKS 代理可能需要认证，因此 curl 需要向代理提供适当的凭据才能使用它。未能这样做（或提供了错误的凭据）将导致代理返回使用代码 407 的 HTTP 响应。

代理认证类似于“正常”的 HTTP 认证。它是独立于服务器认证的，以便客户端可以独立使用正常的宿主认证以及代理认证。

使用 curl 时，你可以通过`-U user:password`或`--proxy-user user:password`选项设置代理认证的用户名和密码：

```sh
curl -U daniel:secr3t -x myproxy:80 http://example.com
```

此示例默认使用基本认证方案。一些代理需要其他认证方案（并且当你收到 407 响应时返回的头部信息会告诉你哪种），然后你可以使用`--proxy-digest`、`--proxy-negotiate`、`--proxy-ntlm`请求特定的方法。再次给出上述示例命令，但这次是请求使用代理的 NTLM 认证：

```sh
curl -U daniel:secr3t -x myproxy:80 http://example.com --proxy-ntlm
```

也有一种选项是让 curl 确定代理希望使用并支持的方法，然后使用`--proxy-anyauth`跟随那个方法（可能会增加额外的往返次数）。让 curl 使用代理想要的任何方法是这样的：

```sh
curl -U daniel:secr3t -x myproxy:80 http://example.com --proxy-anyauth
```
