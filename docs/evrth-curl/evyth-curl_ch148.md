# HTTP 版本

与任何其他互联网协议一样，HTTP 协议在过去的几年里一直在不断发展，现在有客户端和服务器分布在世界各地，随着时间的推移，它们使用不同版本的 HTTP，成功率各不相同。为了使 curl 能够与你的 URL 一起工作，curl 为你提供了指定请求和传输应使用哪个 HTTP 版本的方法。curl 的设计方式是首先尝试使用最常见、最合理（如果你愿意的话）的默认值，但有时这还不够，那么你可能需要指导 curl 如何操作。

curl 默认对 HTTP 服务器使用 HTTP/1.1，但如果连接到 HTTPS 并且你有内置 HTTP/2 功能的 curl，它会自动尝试协商 HTTP/2，或者在协商失败的情况下回退到 1.1。不具备 HTTP/2 功能的 curl 默认通过 HTTPS 获取 1.1。

| 选项 | 描述 |
| --- | --- |
| –http1.0 | HTTP/1.0 |
| –http1.1 | HTTP/1.1 |
| –http2 | HTTP/2 |
| –http2-prior-knowledge | HTTP/2 |
| –http3 | HTTP/3 |

+   HTTP/0.9

+   HTTP/2

+   HTTP/3
