# 内容编码

## 关于内容编码

[HTTP/1.1][4] 规定，客户端可以请求服务器对响应进行编码。这通常用于使用一组常用压缩技术之一（或多个）压缩响应。这些方案包括 `deflate`（zlib 算法）、`gzip`、`br`（brotli）和 `compress`。客户端通过在请求文档中包含 `Accept-Encoding` 报头来请求服务器执行编码。报头的值应该是已识别的令牌之一 `deflate`、…（有一种方法可以注册新的方案/令牌，请参阅规范的第 3.5 节）。服务器可以尊重客户端的编码请求。当响应被编码时，服务器会在响应中包含一个 `Content-Encoding` 报头。`Content-Encoding` 报头的值指示使用了哪些编码来编码数据，以及它们应用的顺序。

客户端还可以对不同方案附加优先级，以便服务器知道它更喜欢哪一个。有关 `Accept-Encoding` 报头的信息，请参阅 RFC 2616 的第 14.3 节。有关 `Content-Encoding` 报头的信息，请参阅 RFC 7231 的第 [3.1.2.2](https://datatracker.ietf.org/doc/html/rfc7231#section-3.1.2.2) 节。

## 支持的内容编码

libcurl 支持 `deflate`、`gzip`、`zstd` 和 `br` 内容编码。常规和分块传输都能正常工作。`deflate` 和 `gzip` 编码需要 zlib 库，`br` 编码需要 brotli 解码库，而 libzstd 用于 `zstd`。

## libcurl 接口

要使 libcurl 请求内容编码，请使用：

使用 `curl_easy_setopt`1

其中 string 是 `Accept-Encoding` 报头的预期值。

目前，libcurl 支持多种编码，但仅了解如何处理使用 `deflate`、`gzip`、`zstd` 和/或 `br` 内容编码的响应，因此除了 `identity`（它什么都不做）之外，`CURLOPT_ACCEPT_ENCODING`[5] 的唯一有效值是 `deflate`、`gzip`、`zstd` 和 `br`。如果响应使用 `compress` 或其他方法编码，libcurl 会返回一个错误，指示响应无法解码。如果 `<string>` 为 NULL，则不生成 `Accept-Encoding` 报头。如果 `<string>` 是零长度字符串，则生成包含所有支持编码的 `Accept-Encoding` 报头。

必须将 `CURLOPT_ACCEPT_ENCODING`[5] 设置为任何非空值，以便自动解码内容。如果没有设置，并且服务器仍然发送编码内容（尽管没有请求），则数据以原始形式返回，并且不检查 `Content-Encoding` 类型。

## curl 接口

使用 curl 的 `--compressed`[6] 选项来指示它请求服务器使用 curl 支持的任何格式压缩响应。
