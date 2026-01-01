# HSTS

HTTP 严格传输安全，HSTS，是一种协议机制，有助于保护 HTTPS 服务器免受中间人攻击，如协议降级攻击和 cookie 劫持。它允许 HTTPS 服务器声明客户端应自动使用仅 HTTPS 连接与该主机名交互 - 并且明确不使用明文协议与它交互。

## HSTS 缓存

某个服务器名称的 HSTS 状态设置在响应头中，并有一个过期时间。每个 HSTS 主机名的状态都需要保存在一个文件中，以便 curl 能够获取它并更新状态和过期时间。

调用 curl 并告诉它使用哪个文件作为 hsts 缓存：

```sh
curl --hsts hsts.txt https://example.com
```

curl 仅在通过安全传输读取头信息时更新 hsts 信息，因此不是在明文协议上完成时更新。

## 使用 HSTS 更新不安全的协议

如果缓存文件现在包含给定主机名的条目，即使你尝试使用不安全的协议连接到它，它也会自动切换到安全协议：

```sh
curl --hsts hsts.txt http://example.com
```
