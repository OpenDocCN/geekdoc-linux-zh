# 依赖项

制作优秀软件的关键是建立在其他优秀软件之上。通过使用许多其他人使用的库，我们减少了对相同事物的重复发明，并且由于有更多的人使用相同的代码，我们得到了更可靠的软件。

curl 提供的许多功能都需要它被构建为使用一个或多个外部库。它们然后成为 curl 的依赖项。它们都不是必需的，但大多数用户希望至少使用其中的一些。

## HTTP 压缩

如果使用适当的第三方库构建 curl，则 curl 可以自动解压缩通过 HTTP 传输的数据。您可以将 curl 构建为使用这些库中的一个或多个：

+   使用 [zlib](https://zlib.net/) 进行 gzip 压缩

+   使用 [brotli](https://github.com/google/brotli) 进行 brotli 压缩

+   使用 [libzstd](https://github.com/facebook/zstd) 进行 zstd 压缩

通过网络获取压缩数据使用更少的带宽，这也可能导致更短的传输时间。

## c-ares

[`c-ares.org/`](https://c-ares.org/)

可以使用 c-ares 来构建 curl 以实现异步名称解析。另一个启用异步名称解析的选项是使用线程化名称解析后端构建 curl，这样就会为每个名称解析创建一个单独的辅助线程。c-ares 在同一个线程中完成所有操作。

## nghttp2

[`nghttp2.org/`](https://nghttp2.org/)

这是一个处理 HTTP/2 封装的库，是 curl 支持 HTTP 版本 2 的先决条件。

## openldap

[`www.openldap.org/`](https://www.openldap.org/)

这个库是允许 curl 支持 LDAP 和 LDAPS URL 方案的选项之一。在 Windows 上，您还可以选择构建使用 winldap 库的 curl。

## librtmp

[`rtmpdump.mplayerhq.hu/`](https://rtmpdump.mplayerhq.hu/)

要启用 curl 对 RTMP URL 方案的支持，必须使用来自 RTMPDump 项目的 librtmp 库构建 curl。

## libpsl

[`rockdaboot.github.io/libpsl/`](https://rockdaboot.github.io/libpsl/)

当您使用 libpsl 支持构建 curl 时，cookie 解析器会了解公共后缀列表，并相应地处理此类 cookie。

## libidn2

[`www.gnu.org/software/libidn/libidn2/manual/libidn2.html`](https://www.gnu.org/software/libidn/libidn2/manual/libidn2.html)

curl 通过 libidn2 库的帮助处理国际域名 (IDN)。

## SSH 库

如果您希望 curl 具有 SCP 和 SFTP 支持，请使用以下 SSH 库之一进行构建：

+   [libssh2](https://libssh2.org/)

+   [libssh](https://www.libssh.org/)

+   [wolfSSH](https://www.wolfssl.com/products/wolfssh/)

## TLS 库

有许多不同的 TLS 库可供选择，因此它们在 单独的部分 中进行了介绍。

## QUIC 和 HTTP/3

要构建支持 HTTP/3 的 curl，您需要以下这些集合之一：

+   [ngtcp2](https://github.com/ngtcp2/ngtcp2) + [nghttp3](https://github.com/ngtcp2/nghttp3)

+   [quiche](https://github.com/cloudflare/quiche) （**实验性**）

+   [msquic](https://github.com/microsoft/msquic) + [msh3](https://github.com/nibanks/msh3) （**实验性**）
