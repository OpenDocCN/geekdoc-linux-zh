# haproxy

虽然 haproxy 协议的名字中仍有代理，但它与其他代理选项不同，并且不会以其他代理选项的方式与代理一起工作。

这是一种客户端将 IP 地址传递给服务器的方式，无论流量如何到达它：隧道、TCP 代理、负载均衡器、透明代理等等。某些服务会在流量最终到达服务器时更改正在使用的源 IP 地址，使得服务器无法自行确定客户端的 IP 地址。

haproxy 协议很简单。它需要服务器的支持，这意味着用户不能仅仅决定使用它，而服务器不愿意或不合作。如果它*确实*支持它，你可以告诉 curl 使用它并将自己的 IP 地址传递给服务器。

## curl 和 haproxy

curl 只支持 haproxy 协议 v1。

要传递当前正在使用的连接的实际 IP 地址，只需添加布尔标志，如下所示：

```sh
curl --haproxy-protocol https://example.com/
```

如果出于某种原因，这样的命令行没有提供您认为应该传递的 IP 地址，您可以使用 IPv4 或 IPv6 数值地址自行指定确切地址：

```sh
curl --haproxy-clientip 10.0.0.1 https://example.com/
curl --haproxy-clientip fe80::fea3:8a22 https://example.com/
```
