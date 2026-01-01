# 版本

像所有互联网协议一样，HTTP 协议在多年中一直在不断发展，现在有客户端和服务器遍布全球，随着时间的推移，它们使用不同版本的协议，成功率各不相同。为了使 libcurl 能够与您传递的 URL 一起工作，libcurl 提供了您指定要使用哪个 HTTP 版本的方法。libcurl 的设计方式是首先尝试使用最常见、最合理的默认值，但有时这还不够，然后您可能需要指示 libcurl 如何操作。

如果您使用具有内置 HTTP/2 功能的 libcurl 构建，libcurl 默认使用 HTTP/2 进行 HTTPS 服务器。然后 libcurl 尝试自动使用 HTTP/2，或者在协商失败的情况下回退到 1.1。不具备 HTTP/2 功能的 libcurl 默认通过 HTTPS 使用 HTTP/1.1。纯 HTTP 请求默认使用 HTTP/1.1。

如果默认行为不足以满足您的传输需求，`CURLOPT_HTTP_VERSION` 选项可供您使用。

| 选项 | 描述 |
| --- | --- |
| CURL_HTTP_VERSION_NONE | 重置回默认行为 |
| CURL_HTTP_VERSION_1_0 | 强制使用旧的 HTTP/1.0 协议版本 |
| CURL_HTTP_VERSION_1_1 | 使用 HTTP/1.1 协议版本进行请求 |
| CURL_HTTP_VERSION_2_0 | 尝试使用 HTTP/2 |
| CURL_HTTP_VERSION_2TLS | 仅在 HTTPS 连接上尝试使用 HTTP/2，否则使用 HTTP/1.1 |
| CURL_HTTP_VERSION_2_PRIOR_KNOWLEDGE | 直接使用 HTTP/2 而不是从 1.1 升级。这要求您知道该服务器可以接受这一点。 |
| CURL_HTTP_VERSION_3 | 尝试使用 HTTP/3，允许回退到较旧版本。 |
| CURL_HTTP_VERSION_3ONLY | 使用 HTTP/3，如果不可能则失败 |

## 版本 2 不是强制性的

当您要求 libcurl 使用 HTTP/2 时，这是一个请求而不是要求。然后 libcurl 允许服务器选择使用 HTTP/1.1 或 HTTP/2，这决定了最终使用的协议。

## 版本 3 可以为强制

当您要求 libcurl 使用 `CURL_HTTP_VERSION_3` 选项进行 HTTP/3 时，它会使 libcurl 进行第二次并行但略微延迟的连接尝试，这样如果 HTTP/3 连接失败，它仍然可以尝试使用较旧的 HTTP 版本。

使用 `CURL_HTTP_VERSION_3ONLY` 表示不使用回退机制，失败的 QUIC 连接将完全失败传输。
