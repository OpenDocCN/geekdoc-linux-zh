# 验证服务器证书

如果你不能确定你正在与正确的宿主进行通信，那么与服务器建立安全连接就没有太大的意义。如果我们不知道这一点，我们可能只是在和一个冒充者交谈，这个冒充者看起来就像是我们认为的那样。

为了检查它与正确的 TLS 服务器通信，curl 使用一个 CA 证书存储库——一组用于验证服务器证书签名的证书。所有服务器都在 TLS 握手过程中向客户端提供一个证书，所有使用公共 TLS 的服务器都从已建立的证书权威机构获得了该证书。

经过一些加密魔术之后，curl 确认服务器确实是获取了 curl 连接到的主机名的证书的正确服务器。未能验证服务器的证书是 TLS 握手失败，curl 会带错误退出。

在罕见的情况下，你可能仍然决定即使证书验证失败，也要与 TLS 服务器进行通信。那么你接受这样一个事实，即你的通信可能会受到中间人攻击。你可以通过 `-k` 或 `--insecure` 选项降低你的安全防护。

## 本地 CA 证书存储库

操作系统如 Windows 和 macOS 通常都有自己的 CA 证书存储库。

如果你使用 Windows 上的 Schannel 运行 curl，curl 默认使用 Windows 的 CA 证书存储库。

如果你使用 macOS 上的 Secure Transport 运行 curl，curl 默认使用 macOS 的 CA 证书存储库。

如果你使用 curl 与 Schannel 或 Secure Transport 之外的任何 TLS 后端，它将使用一个单独的文件或目录中提供的 CA 证书存储库，独立于本地的 CA 证书存储库。然而，对于其中的一些，你仍然可以使用 `--ca-native` 命令行选项让 curl 优先使用本地的 CA 证书存储库。此选项与 OpenSSL（及其分支）、wolfSSL 和 GnuTLS 兼容。

对于 HTTPS 代理，相应的选项称为 `--proxy-ca-native`。

## 文件中的 CA 证书存储库

如果 curl 没有构建为使用你平台本地的 TLS 库（如 Schannel 或 Secure Transport），它必须已经构建为知道本地 CA 证书存储库的位置，或者用户需要在调用 curl 时提供 CA 证书存储库的路径。

你可以使用 `--cacert` 命令行选项指定用于 TLS 握手的特定 CA 证书包。该包需要是 PEM 格式。你也可以设置环境变量 `CURL_CA_BUNDLE` 为完整路径。

## Windows 上的 CA 证书存储库

在 Windows 上构建的未使用本地 TLS 库（Schannel）的 curl，有额外的序列来查找和使用 CA 证书存储库。

curl 在这些目录中按此顺序搜索名为 `curl-ca-bundle.crt` 的 CA 证书文件：

1.  应用程序目录

1.  当前工作目录

1.  Windows 系统目录（例如 `C:\windows\system32`）

1.  Windows 目录（例如 `C:\windows`）

1.  `%PATH%` 中的所有目录
