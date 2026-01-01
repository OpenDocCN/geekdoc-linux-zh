# 传输后信息

记住 libcurl 传输与 *easy 处理句柄* 的关联。每个传输都有一个这样的句柄，当传输完成时，在句柄被清理或用于另一个传输之前，它可以用来从之前的操作中提取信息。

你的朋友 `curl_easy_getinfo()` 可以做这件事，你告诉它你感兴趣的具体信息，如果它能提供，它就会返回。

当你使用此函数时，你需要传入 easy 处理句柄、所需信息和用于存储答案的变量的指针。你必须传入正确类型的变量指针，否则可能会出现意外情况。这些信息值旨在在传输完成后提供。

你接收到的数据可以是一个长整型、`char *`、`struct curl_slist*`、一个双精度浮点数或一个套接字。

这就是如何从之前的 HTTP 传输中提取 `Content-Type:` 值：

```sh
CURLcode res;
char *content_type;
res = curl_easy_getinfo(curl, CURLINFO_CONTENT_TYPE, &content_type);
```

如果你想提取在该连接中使用过的本地端口号：

```sh
CURLcode res;
long port_number;
res = curl_easy_getinfo(curl, CURLINFO_LOCAL_PORT, &port_number);
```

## 可用信息

| 获取信息选项 | 类型 | 描述 |
| --- | --- | --- |
| `CURLINFO_ACTIVESOCKET` | `curl_socket_t` | 会话的活跃套接字 |
| `CURLINFO_APPCONNECT_TIME` | `double` | 从开始到 SSL/SSH 握手完成的时刻 |
| `CURLINFO_APPCONNECT_TIME_T` | `curl_off_t` | 从开始到 SSL/SSH 握手完成的时刻（以微秒为单位） |
| `CURLINFO_CAINFO` | `char *` | libcurl 构建时使用的默认 CA 文件路径 |
| `CURLINFO_CAPATH` | `char *` | libcurl 构建时使用的 CA 目录路径 |
| `CURLINFO_CERTINFO` | `struct curl_slist *` | 证书链 |
| `CURLINFO_CONDITION_UNMET` | `long` | 是否满足时间条件 |
| `CURLINFO_CONNECT_TIME` | `double` | 从开始到远程主机或代理完成的时间 |
| `CURLINFO_CONNECT_TIME_T` | `curl_off_t` | 从开始到远程主机或代理完成的时间（以微秒为单位） |
| `CURLINFO_CONN_ID` | `curl_off_t` | 当前连接的数值 ID（用于回调） |
| `CURLINFO_CONTENT_LENGTH_DOWNLOAD` | `double` | 从 Content-Length 报头获取的内容长度 |
| `CURLINFO_CONTENT_LENGTH_DOWNLOAD_T` | `curl_off_t` | 从 Content-Length 报头获取的内容长度 |
| `CURLINFO_CONTENT_LENGTH_UPLOAD` | `double` | 上传大小 |
| `CURLINFO_CONTENT_LENGTH_UPLOAD_T` | `curl_off_t` | 上传大小 |
| `CURLINFO_CONTENT_TYPE` | `char *` | 从 Content-Type 报头获取的内容类型 |
| `CURLINFO_COOKIELIST` | `struct curl_slist *` | 所有已知 cookie 的列表 |
| `CURLINFO_EFFECTIVE_METHOD` | `char *` | 最后使用的 HTTP 请求方法 |
| `CURLINFO_EFFECTIVE_URL` | `char *` | 最后使用的 URL |
| `CURLINFO_FILETIME` | `long` | 获取的文档的远程时间 |
| `CURLINFO_FILETIME_T` | `curl_off_t` | 获取的文档的远程时间 |
| `CURLINFO_FTP_ENTRY_PATH` | `char *` | 登录 FTP 服务器后的入口路径 |
| `CURLINFO_HEADER_SIZE` | `long` | 接收到的所有报头字节数 |
| `CURLINFO_HTTP_CONNECTCODE` | `long` | 最后代理 CONNECT 响应代码 |
| `CURLINFO_HTTP_VERSION` | `long` | 连接中使用的 HTTP 版本 |
| `CURLINFO_HTTPAUTH_AVAIL` | `long` | 可用的 HTTP 身份验证方法（位掩码） |
| `CURLINFO_LASTSOCKET` | `long` | 最后使用的套接字 |
| `CURLINFO_LOCAL_IP` | `char *` | 上次连接的本地端 IP 地址 |
| `CURLINFO_LOCAL_PORT` | `long` | 上次连接的本地端端口 |
| `CURLINFO_NAMELOOKUP_TIME` | `double` | 从开始到名称解析完成的时间 |
| `CURLINFO_NAMELOOKUP_TIME_T` | `curl_off_t` | 从开始到名称解析完成的时间（微秒） |
| `CURLINFO_NUM_CONNECTS` | `long` | 用于上次传输的新成功连接数 |
| `CURLINFO_OS_ERRNO` | `long` | 上次连接失败的 errno |
| `CURLINFO_PRETRANSFER_TIME` | `double` | 从开始到传输开始之前的时间 |
| `CURLINFO_PRETRANSFER_TIME_T` | `curl_off_t` | 从开始到传输开始之前的时间（微秒） |
| `CURLINFO_PRIMARY_IP` | `char *` | 上次连接的 IP 地址 |
| `CURLINFO_PRIMARY_PORT` | `long` | 上次连接的端口 |
| `CURLINFO_PRIVATE` | `char *` | 用户私有数据指针 |
| `CURLINFO_PROTOCOL` | `long` | 用于连接的协议 |
| `CURLINFO_PROXY_ERROR` | `long` | 如果从传输返回`CURLE_PROXY`，则详细的（SOCKS）代理错误 |
| `CURLINFO_PROXY_SSL_VERIFYRESULT` | `long` | 代理证书验证结果 |
| `CURLINFO_PROXYAUTH_AVAIL` | `long` | 可用的 HTTP 代理身份验证方法 |
| `CURLINFO_QUEUE_TIME_T` | `curl_off_t` | 此传输在队列中等待开始的时间（微秒） |
| `CURLINFO_REDIRECT_COUNT` | `long` | 跟随的重定向总数 |
| `CURLINFO_REDIRECT_TIME` | `double` | 在最终传输之前所有重定向步骤所需的时间 |
| `CURLINFO_REDIRECT_TIME_T` | `curl_off_t` | 在最终传输之前所有重定向步骤所需的时间（微秒） |
| `CURLINFO_REDIRECT_URL` | `char *` | 重定向会带你去到的 URL，如果你启用了重定向 |
| `CURLINFO_REFERER` | `char *` | 使用的请求`Referer:`头 |
| `CURLINFO_REQUEST_SIZE` | `long` | 发出的 HTTP 请求中发送的字节数 |
| `CURLINFO_RESPONSE_CODE` | `long` | 最后接收到的响应代码 |
| `CURLINFO_RETRY_AFTER` | `curl_off_t` | 从响应`Retry-After:`头中获取的值 |
| `CURLINFO_RTSP_CLIENT_CSEQ` | `long` | RTSP 下一次期望的客户 CSeq |
| `CURLINFO_RTSP_CSEQ_RECV` | `long` | RTSP 最后接收 |
| `CURLINFO_RTSP_SERVER_CSEQ` | `long` | RTSP 下一次期望的服务器 CSeq |
| `CURLINFO_RTSP_SESSION_ID` | `char *` | RTSP 会话 ID |
| `CURLINFO_SCHEME` | `char *` | 用于连接的方案 |
| `CURLINFO_SIZE_DOWNLOAD` | `double` | 下载的字节数 |
| `CURLINFO_SIZE_DOWNLOAD_T` | `curl_off_t` | 下载的字节数 |
| `CURLINFO_SIZE_UPLOAD` | `double` | 上传的字节数 |
| `CURLINFO_SIZE_UPLOAD_T` | `curl_off_t` | 上传的字节数 |
| `CURLINFO_SPEED_DOWNLOAD` | `double` | 平均下载速度 |
| `CURLINFO_SPEED_DOWNLOAD_T` | `curl_off_t` | 平均下载速度 |
| `CURLINFO_SPEED_UPLOAD` | `double` | 平均上传速度 |
| `CURLINFO_SPEED_UPLOAD_T` | `curl_off_t` | 平均上传速度 |
| `CURLINFO_SSL_ENGINES` | `struct curl_slist *` | OpenSSL 加密引擎列表 |
| `CURLINFO_SSL_VERIFYRESULT` | `long` | 证书验证结果 |
| `CURLINFO_STARTTRANSFER_TIME` | `double` | 从开始到接收到第一个字节的时刻的时间 |
| `CURLINFO_STARTTRANSFER_TIME_T` | `curl_off_t` | 从开始到接收到第一个字节的时刻的时间（以微秒为单位） |
| `CURLINFO_TLS_SSL_PTR` | `struct curl_slist *` | 可用于进一步处理的 TLS 会话信息 |
| `CURLINFO_TOTAL_TIME` | `double` | 上一次传输的总时间 |
| `CURLINFO_TOTAL_TIME_T` | `curl_off_t` | 上一次传输的总时间（以微秒为单位） |
| `CURLINFO_XFER_ID` | `curl_off_t` | 当前传输的数值 ID（用于回调） |
