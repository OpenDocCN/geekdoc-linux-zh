# TLS 认证

TLS 连接提供了一种（很少使用）称为安全远程密码的功能。使用它，您可以使用用户名和密码以及命令行标志来认证服务器的连接，这些标志是`--tlsuser <name>`和`--tlspassword <secret>`。就像这样：

```sh
curl --tlsuser daniel --tlspassword secret https://example.com
```
