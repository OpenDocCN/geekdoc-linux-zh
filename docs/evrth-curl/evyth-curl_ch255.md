# HSTS

HSTS 是 HTTP Strict-Transport-Security 的缩写。这是一种服务器告知客户端，客户端应优先使用 HTTPS 与该站点进行通信，并在未来指定时间段内使用该站点的定义方式。

下面是如何使用 libcurl 与 HSTS 一起使用的方法。

## 内存缓存

libcurl 主要具有用于 HSTS 主机的内存缓存功能，这样后续只针对 HTTP 的请求到缓存中存在的域名将被内部“重定向”到 HTTPS 版本。假设你已启用此功能。

## 为句柄启用 HSTS

通过使用 `CURLOPT_HSTS_CTRL` 选项并调用 `curl_easy_setopt()` 来设置正确的掩码位，可以启用 HSTS。掩码位有两个独立的标志可以使用，但 `CURLHSTS_ENABLE` 是主要的一个。如果设置了该标志，则此简单句柄已启用 HSTS 支持。

此选项可用的第二个标志是 `CURLHSTS_READONLYFILE`，如果设置此标志，则告诉 libcurl，你指定的用于作为 HSTS 缓存的文件名仅用于读取，不应写入任何内容。

## 设置 HSTS 缓存文件

如果你想在磁盘上持久化 HSTS 缓存，则使用 `CURLOPT_HSTS` 选项设置一个文件名。libcurl 在传输开始时从该文件读取，并在简单句柄关闭时写入它（除非它被设置为只读）。
