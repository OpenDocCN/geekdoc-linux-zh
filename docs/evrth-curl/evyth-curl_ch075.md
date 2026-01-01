# 版本

要了解您安装的 curl 版本，请运行

```sh
curl --version
```

或者使用简写版本：

```sh
curl -V
```

该命令行的输出通常是四行，其中一些可能相当长，可能会在您的终端窗口中换行。

2020 年 6 月 Debian Linux 的一个示例输出：

```sh
curl 7.68.0 (x86_64-pc-linux-gnu) libcurl/7.68.0 OpenSSL/1.1.1g
zlib/1.2.11 brotli/1.0.7 libidn2/2.3.0 libpsl/0.21.0 (+libidn2/2.3.0)
libssh2/1.8.0 nghttp2/1.41.0 librtmp/2.3
Release-Date: 2020-01-08
Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3
pop3s rtmp rtsp scp sftp smb smbs smtp smtps telnet tftp 
Features: AsynchDNS brotli GSS-API HTTP2 HTTPS-proxy IDN IPv6 Kerberos
Largefile libz NTLM NTLM_WB PSL SPNEGO SSL TLS-SRP UnixSockets
```

而在相同日期的 Windows 10 机器上运行的相同命令行看起来像：

```sh
curl 7.55.1 (Windows) libcurl/7.55.1 WinSSL
Release-Date: [unreleased]
Protocols: dict file ftp ftps http https imap imaps pop3 pop3s smtp smtps
telnet tftp
Features: AsynchDNS IPv6 Largefile SSPI Kerberos SPNEGO NTLM SSL
```

这四行的含义是什么？

## 第 1 行：curl

第一行以`curl`开头，首先显示工具的主版本号。然后是工具构建的平台（括号内），以及 libcurl 版本。这三个字段对所有 curl 构建都是通用的。

如果 curl 版本号后面附加了`-DEV`，则表示该版本是从开发源代码直接构建的，并且它不是一个官方发布和认可的版本。

这行剩余部分包含 curl 构建使用的第三方组件名称，通常旁边有斜杠分隔的各自版本号。例如`OpenSSL/1.1.1g`和`nghttp2/1.41.0`。这可以告诉你 curl 使用了哪些 TLS 后端。

### 第 1 行：TLS 版本

第 1 行可能包含一个或多个 TLS 库。curl 可以构建以支持多个 TLS 库，这使得 curl 在启动时选择为这次调用使用哪个特定的后端。

如果 curl 支持多个 TLS 库，则默认未选择的库列在括号内。因此，如果您没有指定要使用哪个后端（使用`CURL_SSL_BACKEND`环境变量），则使用未加括号的列表中的后端。

## 第 2 行：发布日期

这行显示了 curl 版本由 curl 项目发布日期，也可以显示一个次要的“补丁日期”，如果它在最初发布后以某种方式更新过。

这表示如果 curl 不是从发布 tarball 构建的，而是像上面所示的那样，Microsoft 为 Windows 10 做了，curl 项目不推荐这样做。

## 第 3 行：协议

这是 curl 构建支持的按字母顺序排列的所有传输协议（URL 方案）。所有名称均以小写字母显示。

此列表可以包含以下协议：

dict, file, ftp, ftps, gopher, http, https, imap, imaps, ldap, ldaps, mqtt, pop3, pop3s, rtmp, rtsp, scp, sftp, smb, smbs, smtp, smtps, telnet 和 tftp

## 第 4 行：特性

这个 curl 构建支持的特性列表。如果名称出现在列表中，则表示该特性已启用。如果名称不在列表中，则表示该特性未启用。

可能存在的特性：

+   **alt-svc** - 支持`alt-svc:`头

+   **AsynchDNS** - 这个 curl 使用异步域名解析。异步域名解析可以使用 c-ares 或线程解析后端。

+   **brotli** - 支持通过 HTTP(S)自动 brotli 压缩

+   **CharConv** - curl 构建时支持字符集转换（如 EBCDIC）

+   **调试** - 这个 curl 使用的是带有调试的 libcurl 构建的。这可以启用更多的错误跟踪和内存调试等。仅适用于 curl 开发者。

+   **GSS-API** - 启用了 GSS-API 认证。

+   **HTTP2** - 内置了 HTTP/2 支持。

+   **HTTP3** - 内置了 HTTP/3 支持。

+   **HTTPS 代理** - 这个 curl 被构建来支持 HTTPS 代理。

+   **IDN** - 这个 curl 支持 IDN - 国际域名。

+   **IPv6** - 你可以使用 IPv6。

+   **krb4** - 支持 FTP 的 Krb4。

+   **Largefile** - 这个 curl 支持大文件传输，文件大小超过 2GB。

+   **libz** - 支持通过 HTTP 压缩文件的自动 gzip 解压缩。

+   **Metalink** - 这个 curl 支持 Metalink。在现代 curl 版本中，此选项永远不会可用。

+   **MultiSSL** - 这个 curl 支持多个 TLS 后端。第一行详细说明了确切的 TLS 库。

+   **NTLM** - 支持 NTLM 认证。

+   **NTLM_WB** - 支持 NTLM 认证。

+   **PSL** - 公共后缀列表（PSL）可用，这意味着这个 curl 已经内置了关于*公共后缀*的知识，用于 cookie。

+   **SPNEGO** - 支持 SPNEGO 认证。

+   **SSL** - 支持各种协议的 SSL 版本，例如 HTTPS、FTPS、POP3S 等。

+   **SSPI** - 支持 SSPI。

+   **TLS-SRP** - 支持 TLS 的 SRP（安全远程密码）认证。

+   **UnixSockets** - 提供 Unix 套接字支持。
