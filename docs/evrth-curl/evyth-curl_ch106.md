# 网络接口

在拥有多个网络接口且连接到多个网络的机器上，存在这样的情况：你可以决定你希望出站网络流量使用哪个网络接口。或者，在通信中，使用你拥有的多个 IP 地址中的哪一个作为原始 IP 地址。

使用`--interface`选项告诉 curl 你想要将通信的本地端“绑定”到哪个网络接口、哪个 IP 地址，甚至是主机名：

```sh
curl --interface eth1 https://www.example.com/

curl --interface 192.168.0.2 https://www.example.com/

curl --interface machine2 https://www.example.com/
```
