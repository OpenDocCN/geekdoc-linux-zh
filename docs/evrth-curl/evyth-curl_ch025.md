# HTTP 基础

HTTP 是一个易于学习基础的协议。客户端连接到服务器——并且总是客户端采取主动——发送请求并接收响应。请求和响应都由头部和主体组成。在两个方向上可能会有很少或很多信息传输。

客户端发送的 HTTP 请求以请求行开始，随后是头部信息，然后可选地跟一个主体。最常见的一种 HTTP 请求可能是 GET 请求，它要求服务器返回一个特定的资源，并且这种请求不包含主体。

当客户端连接到 ‘example.com’ 并请求 ‘/’ 资源时，它发送一个没有请求主体的 GET 请求：

```sh
GET / HTTP/1.1
User-agent: curl/2000
Host: example.com
```

…服务器可能会响应如下内容，包括响应头部和响应主体（‘hello’）。响应的第一行还包含了响应代码和服务器支持的具体版本：

```sh
HTTP/1.1 200 OK
Server: example-server/1.1
Content-Length: 5
Content-Type: plain/text

hello
```

如果客户端发送一个包含小请求主体（‘hello’）的请求，它可能看起来像这样：

```sh
POST / HTTP/1.1
Host: example.com
User-agent: curl/2000
Content-Length: 5

hello
```

服务器总是会响应 HTTP 请求，除非出现错误。

## 将 URL 转换为请求

因此，当 HTTP 客户端被赋予一个要操作的 URL 时，该 URL 被使用，分解，这些部分被用于向服务器发送的出站请求的各个地方。让我们以一个示例 URL 为例：

```sh
https://www.example.com/path/to/file
```

+   **https** 表示 curl 使用 TLS 连接到远程端口 443（这是在 URL 中未指定端口时的默认端口号）。

+   **www.example.com** 是 curl 解析到的主机名，它会解析为一个或多个 IP 地址以进行连接。这个主机名也用于 HTTP 请求中的 `Host:` 头部。

+   **/path/to/file** 在 HTTP 请求中用于告诉服务器 curl 想要获取的确切文档/资源
