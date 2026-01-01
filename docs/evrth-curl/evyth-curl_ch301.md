# 解析主机名

即 `hostip.c` 解释

在阅读 `host*.c` 源文件时需要记住的主要编译时定义是这些：

## `CURLRES_IPV6`

此主机具有 `getaddrinfo()` 和家族，因此我们使用它。该主机可能无法解析 IPv6，但我们实际上不必考虑这一点。未启用 IPv6 的主机定义了 `CURLRES_IPV4`。

## `CURLRES_ARES`

如果 libcurl 构建为使用 c-ares 进行异步名称解析，则定义。这可能是在 Windows 或 *nix 上。

## `CURLRES_THREADED`

如果 libcurl 构建为使用线程进行异步名称解析，则定义。名称解析在新的线程中完成，支持的异步 API 与 ares 构建相同。这是在 (本地) Windows 下的默认设置。

如果定义了上述两个中的任何一个，则也会定义 `CURLRES_ASYNCH`。如果 libcurl 没有构建为使用异步解析器，则定义 `CURLRES_SYNCH`。

## `host*.c` 源文件

`host*.c` 源文件是这样分割的：

+   `hostip.c` - 通用解析函数和实用函数

+   `hostasyn.c` - 异步名称解析的函数

+   `hostsyn.c` - 同步名称解析的函数

+   `asyn-ares.c` - 使用 c-ares 进行异步名称解析的函数

+   `asyn-thread.c` - 使用线程进行异步名称解析的函数

+   `hostip4.c` - IPv4 特定函数

+   `hostip6.c` - IPv6 特定函数

`hostip.h` 是所有这些内容的单一联合头文件。它基于 `config*.h` 和 `curl_setup.h` 的定义来定义 `CURLRES_*` 定义。
