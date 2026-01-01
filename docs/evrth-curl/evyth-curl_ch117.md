# SOCKS 代理

SOCKS 是一种用于代理的协议，curl 支持它。curl 支持 SOCKS 版本 4 和版本 5，两种版本都有两种风味。

您可以通过使用给定的代理主机正确的方案部分来选择要使用的特定 SOCKS 版本，使用`-x`，或者您可以使用单独的选项来指定它，而不是`-x`。

SOCKS4 适用于版本 4，但 curl 会解析名称：

```sh
curl -x socks4://proxy.example.com http://www.example.com/

curl --socks4 proxy.example.com http://www.example.com/
```

SOCKS4a 适用于版本 4，解析由代理完成：

```sh
curl -x socks4a://proxy.example.com http://www.example.com/

curl --socks4a proxy.example.com http://www.example.com/
```

SOCKS5 适用于版本 5，而 SOCKS5-hostname 适用于版本 5 且不本地解析主机名：

```sh
curl -x socks5://proxy.example.com http://www.example.com/

curl --socks5 proxy.example.com http://www.example.com/
```

SOCKS5-hostname 版本。这会将主机名发送到代理，因此 curl 在本地不会进行名称解析：

```sh
curl -x socks5h://proxy.example.com http://www.example.com/

curl --socks5-hostname proxy.example.com http://www.example.com/
```

有助于确定哪个版本由哪一侧解析名称的有用表格：

| SOCKS | 谁解析名称 | 支持 IPv6 |
| --- | --- | --- |
| 4 | curl | 否 |
| 4a | 代理 | 否 |
| 5 | curl | 是 |
| 5h | 代理 | 是 |
