# 所有选项

这是`curl_easy_setopt()`可用的选项表。

| 选项 | 目的 |
| --- | --- |
| `CURLOPT_ABSTRACT_UNIX_SOCKET` | 抽象 Unix 域套接字 |
| `CURLOPT_ACCEPT_ENCODING` | 自动解压缩 HTTP 下载 |
| `CURLOPT_ACCEPTTIMEOUT_MS` | 等待 FTP 服务器回连的超时时间 |
| `CURLOPT_ADDRESS_SCOPE` | IPv6 地址的作用域 ID |
| `CURLOPT_ALTSVC_CTRL` | 控制 alt-svc 行为 |
| `CURLOPT_ALTSVC` | alt-svc 缓存文件名 |
| `CURLOPT_APPEND` | 追加到远程文件 |
| `CURLOPT_AUTOREFERER` | 自动更新引用头 |
| `CURLOPT_AWS_SIGV4` | V4 签名 |
| `CURLOPT_BUFFERSIZE` | 接收缓冲区大小 |
| `CURLOPT_CA_CACHE_TIMEOUT` | 缓存证书存储的生命周期 |
| `CURLOPT_CAINFO_BLOB` | PEM 格式的证书颁发机构（CA）包 |
| `CURLOPT_CAINFO` | 证书颁发机构（CA）包的路径 |
| `CURLOPT_CAPATH` | 存放 CA 证书的目录 |
| `CURLOPT_CERTINFO` | 请求 SSL 证书信息 |
| `CURLOPT_CHUNK_BGN_FUNCTION` | FTP 通配符匹配传输前的回调 |
| `CURLOPT_CHUNK_DATA` | 传递给 FTP 块回调的指针 |
| `CURLOPT_CHUNK_END_FUNCTION` | FTP 通配符匹配传输后的回调 |
| `CURLOPT_CLOSESOCKETDATA` | 传递给套接字关闭回调的指针 |
| `CURLOPT_CLOSESOCKETFUNCTION` | 替换套接字关闭的回调 |
| `CURLOPT_CONNECT_ONLY` | 连接到目标服务器后停止 |
| `CURLOPT_CONNECT_TO` | 连接到指定的主机和端口，而不是 URL 的主机和端口 |
| `CURLOPT_CONNECTTIMEOUT_MS` | 连接阶段的超时时间 |
| `CURLOPT_CONNECTTIMEOUT` | 连接阶段超时时间 |
| `CURLOPT_CONV_FROM_NETWORK_FUNCTION` | 将数据从网络编码转换为主机编码 |
| `CURLOPT_CONV_FROM_UTF8_FUNCTION` | 将数据从 UTF8 转换为主机编码 |
| `CURLOPT_CONV_TO_NETWORK_FUNCTION` | 将数据从主机编码转换为网络编码 |
| `CURLOPT_COOKIE` | HTTP Cookie 头 |
| `CURLOPT_COOKIEFILE` | 从中读取 cookie 的文件名 |
| `CURLOPT_COOKIEJAR` | 存储 cookie 的文件名 |
| `CURLOPT_COOKIELIST` | 添加或操作内存中持有的 cookie |
| `CURLOPT_COOKIESESSION` | 开始一个新的 cookie 会话 |
| `CURLOPT_COPYPOSTFIELDS` | 让 libcurl 将数据复制到 POST |
| `CURLOPT_CRLF` | CRLF 转换 |
| `CURLOPT_CRLFILE` | 证书吊销列表文件 |
| `CURLOPT_CURLU` | CURLU *格式的 URL |
| `CURLOPT_CUSTOMREQUEST` | 自定义请求方法 |
| `CURLOPT_DEBUGDATA` | 传递给调试回调的指针 |
| `CURLOPT_DEBUGFUNCTION` | 调试回调函数 |
| `CURLOPT_DEFAULT_PROTOCOL` | 如果 URL 缺少协议，则使用默认协议 |
| `CURLOPT_DIRLISTONLY` | 仅请求目录列表中的名称 |
| `CURLOPT_DISALLOW_USERNAME_IN_URL` | 禁止在 URL 中指定用户名 |
| `CURLOPT_DNS_CACHE_TIMEOUT` | DNS 缓存条目的生命周期 |
| `CURLOPT_DNS_INTERFACE` | 在其上说话 DNS 的接口 |
| `CURLOPT_DNS_LOCAL_IP4` | 绑定 DNS 解析到的 IPv4 地址 |
| `CURLOPT_DNS_LOCAL_IP6` | 绑定 DNS 解析到的 IPv6 地址 |
| `CURLOPT_DNS_SERVERS` | 要使用的 DNS 服务器 |
| `CURLOPT_DNS_SHUFFLE_ADDRESSES` | 打乱主机名的 IP 地址 |
| `CURLOPT_DNS_USE_GLOBAL_CACHE` | 全局 DNS 缓存 |
| `CURLOPT_DOH_SSL_VERIFYHOST` | 验证 DoH SSL 证书中的主机名 |
| `CURLOPT_DOH_SSL_VERIFYPEER` | 验证 DoH SSL 证书 |
| `CURLOPT_DOH_SSL_VERIFYSTATUS` | 验证 DoH SSL 证书的状态 |
| `CURLOPT_DOH_URL` | 提供 DNS-over-HTTPS URL |
| `CURLOPT_EGDSOCKET` | EGD 套接字路径 |
| `CURLOPT_ERRORBUFFER` | 错误消息的错误缓冲区 |
| `CURLOPT_EXPECT_100_TIMEOUT_MS` | Expect: 100-continue 响应的超时时间 |
| `CURLOPT_FAILONERROR` | HTTP 响应 >= 400 时请求失败 |
| `CURLOPT_FILETIME` | 获取远程资源的修改时间 |
| `CURLOPT_FNMATCH_DATA` | 传递给 fnmatch 回调的指针 |
| `CURLOPT_FNMATCH_FUNCTION` | 通配符匹配回调 |
| `CURLOPT_FOLLOWLOCATION` | 跟随 HTTP 3xx 重定向 |
| `CURLOPT_FORBID_REUSE` | 使用后立即关闭连接 |
| `CURLOPT_FRESH_CONNECT` | 强制使用新的连接 |
| `CURLOPT_FTP_ACCOUNT` | FTP 的账户信息 |
| `CURLOPT_FTP_ALTERNATIVE_TO_USER` | 用于 FTP 的替代 USER 命令 |
| `CURLOPT_FTP_CREATE_MISSING_DIRS` | 为 FTP 和 SFTP 创建缺失的目录 |
| `CURLOPT_FTP_FILEMETHOD` | 选择 FTP 目录遍历方法 |
| `CURLOPT_FTP_RESPONSE_TIMEOUT` | 等待 FTP 响应的时间 |
| `CURLOPT_FTP_SKIP_PASV_IP` | 忽略 PASV 响应中的 IP 地址 |
| `CURLOPT_FTP_SSL_CCC` | 在 FTP 身份验证后再次关闭 SSL |
| `CURLOPT_FTP_USE_EPRT` | 为 FTP 使用 EPRT |
| `CURLOPT_FTP_USE_EPSV` | 为 FTP 使用 EPSV |
| `CURLOPT_FTP_USE_PRET` | 为 FTP 使用 PRET |
| `CURLOPT_FTPPORT` | 使 FTP 传输处于活动状态 |
| `CURLOPT_FTPSSLAUTH` | 尝试 TLS 与 SSL 的顺序 |
| `CURLOPT_GSSAPI_DELEGATION` | 允许 GSS-API 委派 |
| `CURLOPT_HAPPY_EYEBALLS_TIMEOUT_MS` | 为 happy eyeballs 的 ipv6 提供提前启动 |
| `CURLOPT_HAPROXY_CLIENT_IP` | 设置 HAProxy PROXY 协议客户端 IP |
| `CURLOPT_HAPROXYPROTOCOL` | 发送 HAProxy PROXY 协议 v1 头部 |
| `CURLOPT_HEADER` | 将头部传递到数据流 |
| `CURLOPT_HEADERDATA` | 传递给头部回调的指针 |
| `CURLOPT_HEADERFUNCTION` | 接收头部数据的回调 |
| `CURLOPT_HEADEROPT` | 将 HTTP 头部发送到代理和主机或分别发送 |
| `CURLOPT_HSTS_CTRL` | 控制 HSTS 行为 |
| `CURLOPT_HSTS` | HSTS 缓存文件名 |
| `CURLOPT_HSTSREADDATA` | 传递给 HSTS 读取回调的指针 |
| `CURLOPT_HSTSREADFUNCTION` | HSTS 主机的读取回调 |
| `CURLOPT_HSTSWRITEDATA` | 传递给 HSTS 写入回调的指针 |
| `CURLOPT_HSTSWRITEFUNCTION` | HSTS 主机的写入回调 |
| `CURLOPT_HTTP09_ALLOWED` | 允许 HTTP/0.9 响应 |
| `CURLOPT_HTTP200ALIASES` | HTTP 200 OK 的替代匹配 |
| `CURLOPT_HTTP_CONTENT_DECODING` | HTTP 内容解码控制 |
| `CURLOPT_HTTP_TRANSFER_DECODING` | HTTP 传输解码控制 |
| `CURLOPT_HTTP_VERSION` | 要使用的 HTTP 协议版本 |
| `CURLOPT_HTTPAUTH` | 尝试的 HTTP 服务器认证方法 |
| `CURLOPT_HTTPGET` | 请求 HTTP GET 请求 |
| `CURLOPT_HTTPHEADER` | HTTP 头集合 |
| `CURLOPT_HTTPPOST` | 表单 post 内容的多部分 |
| `CURLOPT_HTTPPROXYTUNNEL` | 通过 HTTP 代理隧道 |
| `CURLOPT_IGNORE_CONTENT_LENGTH` | 忽略内容长度 |
| `CURLOPT_INFILESIZE_LARGE` | 要发送的输入文件的大小 |
| `CURLOPT_INFILESIZE` | 要发送的输入文件的大小 |
| `CURLOPT_INTERFACE` | 出站流量的源接口 |
| `CURLOPT_INTERLEAVEDATA` | 传递给 RTSP 交错回调的指针 |
| `CURLOPT_INTERLEAVEFUNCTION` | RTSP 交错数据的回调 |
| `CURLOPT_IOCTLDATA` | 指针传递给 I/O 回调 |
| `CURLOPT_IOCTLFUNCTION` | I/O 操作的回调 |
| `CURLOPT_IPRESOLVE` | 要使用的 IP 协议版本 |
| `CURLOPT_ISSUERCERT_BLOB` | 从内存 blob 中获取的发行者 SSL 证书 |
| `CURLOPT_ISSUERCERT` | 发行者 SSL 证书文件名 |
| `CURLOPT_KEEP_SENDING_ON_ERROR` | 在早期 HTTP 响应 >= 300 时继续发送 |
| `CURLOPT_KEYPASSWD` | 私钥的密码短语 |
| `CURLOPT_KRBLEVEL` | FTP Kerberos 安全级别 |
| `CURLOPT_LOCALPORT` | 用于套接字的本地端口号 |
| `CURLOPT_LOCALPORTRANGE` | 尝试的附加本地端口号数量 |
| `CURLOPT_LOGIN_OPTIONS` | 登录选项 |
| `CURLOPT_LOW_SPEED_LIMIT` | 每秒字节数的低速限制 |
| `CURLOPT_LOW_SPEED_TIME` | 低速限制时间周期 |
| `CURLOPT_MAIL_AUTH` | SMTP 认证地址 |
| `CURLOPT_MAIL_FROM` | SMTP 发件人地址 |
| `CURLOPT_MAIL_RCPT_ALLOWFAILS` | 允许 RCPT TO 命令对某些收件人失败 |
| `CURLOPT_MAIL_RCPT` | SMTP 邮件收件人列表 |
| `CURLOPT_MAX_RECV_SPEED_LARGE` | 数据下载速度限制率 |
| `CURLOPT_MAX_SEND_SPEED_LARGE` | 数据上传速度限制率 |
| `CURLOPT_MAXAGE_CONN` | 允许重用连接的最大空闲时间 |
| `CURLOPT_MAXCONNECTS` | 最大连接缓存大小 |
| `CURLOPT_MAXFILESIZE_LARGE` | 允许下载的最大文件大小 |
| `CURLOPT_MAXFILESIZE` | 允许下载的最大文件大小 |
| `CURLOPT_MAXLIFETIME_CONN` | 允许重用连接的最大生命周期（自创建以来） |
| `CURLOPT_MAXREDIRS` | 允许的最大重定向次数 |
| `CURLOPT_MIME_OPTIONS` | 设置 MIME 选项标志 |
| `CURLOPT_MIMEPOST` | 从 MIME 结构发送数据 |
| `CURLOPT_NETRC_FILE` | 从中读取 .netrc 信息的文件名 |
| `CURLOPT_NETRC` | 启用 .netrc 的使用 |
| `CURLOPT_NEW_DIRECTORY_PERMS` | 远程创建目录的权限 |
| `CURLOPT_NEW_FILE_PERMS` | 远程创建文件的权限 |
| `CURLOPT_NOBODY` | 执行下载请求而不获取主体 |
| `CURLOPT_NOPROGRESS` | 关闭进度表 |
| `CURLOPT_NOPROXY` | 禁用特定主机的代理使用 |
| `CURLOPT_NOSIGNAL` | 跳过所有信号处理 |
| `CURLOPT_OPENSOCKETDATA` | 传递给打开套接字回调的指针 |
| `CURLOPT_OPENSOCKETFUNCTION` | 打开套接字的回调 |
| `CURLOPT_PASSWORD` | 用于认证的密码 |
| `CURLOPT_PATH_AS_IS` | 不处理点号序列 |
| `CURLOPT_PINNEDPUBLICKEY` | 固定公钥 |
| `CURLOPT_PIPEWAIT` | 等待管道/多路复用 |
| `CURLOPT_PORT` | 要连接的远程端口号 |
| `CURLOPT_POST` | 执行 HTTP POST |
| `CURLOPT_POSTFIELDS` | 要发送到服务器的数据 |
| `CURLOPT_POSTFIELDSIZE_LARGE` | 指向的 POST 数据的大小 |
| `CURLOPT_POSTFIELDSIZE` | 指向的 POST 数据的大小 |
| `CURLOPT_POSTQUOTE` | 传输后运行的（S）FTP 命令 |
| `CURLOPT_POSTREDIR` | 对 HTTP POST 重定向的处理方式 |
| `CURLOPT_PRE_PROXY` | 要使用的预代理主机 |
| `CURLOPT_PREQUOTE` | FTP 传输前的命令 |
| `CURLOPT_PREREQDATA` | 传递给预请求回调的指针 |
| `CURLOPT_PREREQFUNCTION` | 当连接建立时用户回调函数被调用 |
| `CURLOPT_PRIVATE` | 存储一个私有指针 |
| `CURLOPT_PROGRESSDATA` | 传递给进度回调的指针 |
| `CURLOPT_PROGRESSFUNCTION` | 进度表回调 |
| `CURLOPT_PROTOCOLS_STR` | 允许的协议 |
| `CURLOPT_PROTOCOLS` | 允许的协议 |
| `CURLOPT_PROXY_CAINFO_BLOB` | 以 PEM 格式存储的代理证书颁发机构（CA）证书包 |
| `CURLOPT_PROXY_CAINFO` | 代理证书颁发机构（CA）证书包的路径 |
| `CURLOPT_PROXY_CAPATH` | 存放 HTTPS 代理 CA 证书的目录 |
| `CURLOPT_PROXY_CRLFILE` | HTTPS 代理证书吊销列表文件 |
| `CURLOPT_PROXY_ISSUERCERT_BLOB` | 从内存 blob 中获取的代理发行者 SSL 证书 |
| `CURLOPT_PROXY_ISSUERCERT` | 代理发行者 SSL 证书的文件名 |
| `CURLOPT_PROXY_KEYPASSWD` | 代理私钥的密码短语 |
| `CURLOPT_PROXY_PINNEDPUBLICKEY` | https 代理的固定公钥 |
| `CURLOPT_PROXY_SERVICE_NAME` | 代理认证服务名称 |
| `CURLOPT_PROXY_SSL_CIPHER_LIST` | 用于 HTTPS 代理的加密套件 |
| `CURLOPT_PROXY_SSL_OPTIONS` | HTTPS 代理 SSL 行为选项 |
| `CURLOPT_PROXY_SSL_VERIFYHOST` | 将代理证书的名称与主机进行验证 |
| `CURLOPT_PROXY_SSL_VERIFYPEER` | 验证代理的 SSL 证书 |
| `CURLOPT_PROXY_SSLCERT_BLOB` | 从内存 blob 中获取的 SSL 代理客户端证书 |
| `CURLOPT_PROXY_SSLCERT` | HTTPS 代理客户端证书 |
| `CURLOPT_PROXY_SSLCERTTYPE` | 代理客户端 SSL 证书的类型 |
| `CURLOPT_PROXY_SSLKEY_BLOB` | 从内存 blob 中获取的代理证书的私钥 |
| `CURLOPT_PROXY_SSLKEY` | HTTPS 代理客户端证书的私钥文件 |
| `CURLOPT_PROXY_SSLKEYTYPE` | 代理私钥文件的类型 |
| `CURLOPT_PROXY_SSLVERSION` | 偏好的 HTTPS 代理 TLS 版本 |
| `CURLOPT_PROXY_TLS13_CIPHERS` | 代理 TLS 1.3 的加密套件 |
| `CURLOPT_PROXY_TLSAUTH_PASSWORD` | 用于代理 TLS 认证的密码 |
| `CURLOPT_PROXY_TLSAUTH_TYPE` | HTTPS 代理 TLS 认证方法 |
| `CURLOPT_PROXY_TLSAUTH_USERNAME` | 用于代理 TLS 认证的用户名 |
| `CURLOPT_PROXY_TRANSFER_MODE` | 将 FTP 传输模式附加到代理 URL |
| `CURLOPT_PROXY` | 要使用的代理 |
| `CURLOPT_PROXYAUTH` | HTTP 代理认证方法 |
| `CURLOPT_PROXYHEADER` | 要传递给代理的 HTTP 头部集合 |
| `CURLOPT_PROXYPASSWORD` | 用于代理认证的密码 |
| `CURLOPT_PROXYPORT` | 代理监听的端口号 |
| `CURLOPT_PROXYTYPE` | 代理协议类型 |
| `CURLOPT_PROXYUSERNAME` | 用于代理认证的用户名 |
| `CURLOPT_PROXYUSERPWD` | 用于代理认证的用户名和密码 |
| `CURLOPT_PUT` | 发送 HTTP PUT 请求 |
| `CURLOPT_QUICK_EXIT` | 允许快速退出 |
| `CURLOPT_QUOTE` | 传输前运行的 (S)FTP 命令 |
| `CURLOPT_RANDOM_FILE` | 从中读取随机数据的文件 |
| `CURLOPT_RANGE` | 请求的字节范围 |
| `CURLOPT_READDATA` | 传递给读取回调的指针 |
| `CURLOPT_READFUNCTION` | 数据上传的读取回调 |
| `CURLOPT_REDIR_PROTOCOLS_STR` | 允许重定向到的协议 |
| `CURLOPT_REDIR_PROTOCOLS` | 允许重定向到的协议 |
| `CURLOPT_REFERER` | HTTP 引用头 |
| `CURLOPT_REQUEST_TARGET` | 此请求的替代目标 |
| `CURLOPT_RESOLVE` | 提供自定义主机名到 IP 地址解析 |
| `CURLOPT_RESOLVER_START_DATA` | 传递给 resolver start 回调的指针 |
| `CURLOPT_RESOLVER_START_FUNCTION` | 在开始新的名称解析之前调用的回调 |
| `CURLOPT_RESUME_FROM_LARGE` | 从何处恢复传输的偏移量 |
| `CURLOPT_RESUME_FROM` | 从何处恢复传输的偏移量 |
| `CURLOPT_RTSP_CLIENT_CSEQ` | RTSP 客户端 CSEQ 数字 |
| `CURLOPT_RTSP_REQUEST` | RTSP 请求 |
| `CURLOPT_RTSP_SERVER_CSEQ` | RTSP 服务器 CSEQ 数字 |
| `CURLOPT_RTSP_SESSION_ID` | RTSP 会话 ID |
| `CURLOPT_RTSP_STREAM_URI` | RTSP 流 URI |
| `CURLOPT_RTSP_TRANSPORT` | RTSP 传输：头部 |
| `CURLOPT_SASL_AUTHZID` | 授权身份（要充当的身份） |
| `CURLOPT_SASL_IR` | 在第一个数据包中发送初始响应 |
| `CURLOPT_SEEKDATA` | 传递给 seek 回调的指针 |
| `CURLOPT_SEEKFUNCTION` | 输入流中查找的用户回调 |
| `CURLOPT_SERVER_RESPONSE_TIMEOUT` | 允许等待服务器响应的秒数 |
| `CURLOPT_SERVER_RESPONSE_TIMEOUT_MS` | 允许等待服务器响应的毫秒数 |
| `CURLOPT_SERVICE_NAME` | 认证服务名称 |
| `CURLOPT_SHARE` | 要使用的共享句柄 |
| `CURLOPT_SOCKOPTDATA` | 传递给 sockopt 回调的指针 |
| `CURLOPT_SOCKOPTFUNCTION` | 设置套接字选项的回调 |
| `CURLOPT_SOCKS5_AUTH` | SOCKS5 代理认证方法 |
| `CURLOPT_SOCKS5_GSSAPI_NEC` | socks 代理 gssapi 协商保护 |
| `CURLOPT_SOCKS5_GSSAPI_SERVICE` | SOCKS5 代理认证服务名称 |
| `CURLOPT_SSH_AUTH_TYPES` | SFTP 和 SCP 的认证类型 |
| `CURLOPT_SSH_COMPRESSION` | 启用 SSH 压缩 |
| `CURLOPT_SSH_HOST_PUBLIC_KEY_MD5` | SSH 服务器公钥的 MD5 校验和 |
| `CURLOPT_SSH_HOST_PUBLIC_KEY_SHA256` | SSH 服务器公钥的 SHA256 哈希值 |
| `CURLOPT_SSH_HOSTKEYDATA` | 传递给 SSH 主机密钥回调的指针 |
| `CURLOPT_SSH_HOSTKEYFUNCTION` | 检查主机密钥的回调 |
| `CURLOPT_SSH_KEYDATA` | 传递给 SSH 密钥回调的指针 |
| `CURLOPT_SSH_KEYFUNCTION` | 已知主机匹配逻辑的回调 |
| `CURLOPT_SSH_KNOWNHOSTS` | 包含 SSH 已知主机的文件名 |
| `CURLOPT_SSH_PRIVATE_KEYFILE` | SSH 认证的私钥文件 |
| `CURLOPT_SSH_PUBLIC_KEYFILE` | SSH 认证的公钥文件 |
| `CURLOPT_SSL_CIPHER_LIST` | 用于 TLS 的加密套件 |
| `CURLOPT_SSL_CTX_DATA` | 传递给 ssl_ctx 回调的指针 |
| `CURLOPT_SSL_CTX_FUNCTION` | OpenSSL、wolfSSL 或 mbedTLS 的 SSL 上下文回调 |
| `CURLOPT_SSL_EC_CURVES` | 密钥交换曲线 |
| `CURLOPT_SSL_ENABLE_ALPN` | 应用层协议协商 |
| `CURLOPT_SSL_ENABLE_NPN` | 使用 NPN |
| `CURLOPT_SSL_FALSESTART` | TLS false start |
| `CURLOPT_SSL_OPTIONS` | SSL 行为选项 |
| `CURLOPT_SSL_SESSIONID_CACHE` | 使用 SSL 会话 ID 缓存 |
| `CURLOPT_SSL_VERIFYHOST` | 将证书的名称与主机进行验证 |
| `CURLOPT_SSL_VERIFYPEER` | 验证对等方的 SSL 证书 |
| `CURLOPT_SSL_VERIFYSTATUS` | 验证证书的状态 |
| `CURLOPT_SSLCERT_BLOB` | 从内存 blob 中获取 SSL 客户端证书 |
| `CURLOPT_SSLCERT` | SSL 客户端证书 |
| `CURLOPT_SSLCERTTYPE` | 客户端 SSL 证书的类型 |
| `CURLOPT_SSLENGINE_DEFAULT` | 设置 SSL 引擎为默认 |
| `CURLOPT_SSLENGINE` | SSL 引擎标识符 |
| `CURLOPT_SSLKEY_BLOB` | 从内存 blob 中获取客户端证书的私钥 |
| `CURLOPT_SSLKEY` | TLS 和 SSL 客户端证书的私钥文件 |
| `CURLOPT_SSLKEYTYPE` | 私钥文件的类型 |
| `CURLOPT_SSLVERSION` | 优先的 TLS/SSL 版本 |
| `CURLOPT_STDERR` | 将 stderr 重定向到另一个流 |
| `CURLOPT_STREAM_DEPENDS_E` | 此传输专一依赖的流 |
| `CURLOPT_STREAM_DEPENDS` | 此传输依赖的流 |
| `CURLOPT_STREAM_WEIGHT` | 数值流权重 |
| `CURLOPT_SUPPRESS_CONNECT_HEADERS` | 从用户回调中抑制代理 CONNECT 响应头 |
| `CURLOPT_TCP_FASTOPEN` | TCP 快速打开 |
| `CURLOPT_TCP_KEEPALIVE` | TCP 保活探测 |
| `CURLOPT_TCP_KEEPIDLE` | TCP 保活空闲时间等待 |
| `CURLOPT_TCP_KEEPINTVL` | TCP 保活间隔 |
| `CURLOPT_TCP_NODELAY` | TCP_NODELAY 选项 |
| `CURLOPT_TELNETOPTIONS` | 一组 telnet 选项 |
| `CURLOPT_TFTP_BLKSIZE` | TFTP 块大小 |
| `CURLOPT_TFTP_NO_OPTIONS` | 不发送 TFTP 选项请求 |
| `CURLOPT_TIMECONDITION` | 时间请求的选取条件 |
| `CURLOPT_TIMEOUT_MS` | 允许传输完成的最大时间 |
| `CURLOPT_TIMEOUT` | 允许传输完成的最大时间 |
| `CURLOPT_TIMEVALUE_LARGE` | 条件性时间值 |
| `CURLOPT_TIMEVALUE` | 条件性时间值 |
| `CURLOPT_TLS13_CIPHERS` | 用于 TLS 1.3 的加密套件 |
| `CURLOPT_TLSAUTH_PASSWORD` | 用于 TLS 身份验证的密码 |
| `CURLOPT_TLSAUTH_TYPE` | TLS 身份验证方法 |
| `CURLOPT_TLSAUTH_USERNAME` | 用于 TLS 身份验证的用户名 |
| `CURLOPT_TRAILERDATA` | 传递给尾部头回调的指针 |
| `CURLOPT_TRAILERFUNCTION` | 发送尾部头的回调函数 |
| `CURLOPT_TRANSFER_ENCODING` | 请求 HTTP 传输编码 |
| `CURLOPT_TRANSFERTEXT` | 请求基于文本的 FTP 传输 |
| `CURLOPT_UNIX_SOCKET_PATH` | Unix 域套接字 |
| `CURLOPT_UNRESTRICTED_AUTH` | 向其他主机发送凭据 |
| `CURLOPT_UPKEEP_INTERVAL_MS` | 连接维护间隔 |
| `CURLOPT_UPLOAD_BUFFERSIZE` | 上传缓冲区大小 |
| `CURLOPT_UPLOAD` | 数据上传 |
| `CURLOPT_URL` | 此传输的 URL |
| `CURLOPT_USE_SSL` | 使用 SSL / TLS 进行传输请求 |
| `CURLOPT_USERAGENT` | HTTP 用户代理头 |
| `CURLOPT_USERNAME` | 用于身份验证的用户名 |
| `CURLOPT_USERPWD` | 用于身份验证的用户名和密码 |
| `CURLOPT_VERBOSE` | 详细模式 |
| `CURLOPT_WILDCARDMATCH` | 目录通配符传输 |
| `CURLOPT_WRITEDATA` | 传递给写回调的指针 |
| `CURLOPT_WRITEFUNCTION` | 写入接收数据的回调函数 |
| `CURLOPT_WS_OPTIONS` | WebSocket 行为选项 |
| `CURLOPT_XFERINFODATA` | 传递给进度回调的指针 |
| `CURLOPT_XFERINFOFUNCTION` | 进度计回调函数 |
| `CURLOPT_XOAUTH2_BEARER` | OAuth 2.0 访问令牌 |
