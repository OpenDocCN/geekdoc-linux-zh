# 代理头信息

当你想添加专门针对 HTTP 或 HTTPS 代理的 HTTP 头信息，而不是针对远程服务器时，`--header` 选项就不够用了。

例如，如果你通过 HTTP 代理发出 HTTPS 请求，它首先会向代理发出一个 `CONNECT` 命令，以建立到远程服务器的隧道，然后向该服务器发送请求。这个最初的 `CONNECT` 命令只针对代理，你可能想确保只有它接收你的特殊头信息，并且向远程服务器发送另一组自定义头信息。

只为代理设置特定的不同 `User-Agent:`

```sh
curl --proxy-header "User-Agent: magic/3000" -x proxy https://example.com/
```
