# 分块编码的 POST

当与 HTTP 1.1 服务器通信时，你可以告诉 curl 在发送请求体之前不使用 `Content-Length:` 头部来指定 POST 的确切大小。通过坚持让 curl 使用分块传输编码，curl 会以特殊的方式逐块发送 POST，同时也会在发送过程中发送每个这样的块的大小。

你可以使用 curl 发送分块 POST，如下所示：

```sh
curl -H "Transfer-Encoding: chunked" -d @file http://example.com
```

## 注意事项

这假设你知道你是在与一个 HTTP/1.1 服务器进行交互。在 1.1 之前，没有分块编码，而在 1.1 版本之后，分块编码已经被弃用。
