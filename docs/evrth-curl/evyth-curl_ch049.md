# TLS 库

要使 curl 支持基于 TLS 的协议，例如 HTTPS、FTPS、SMTPS、POP3S、IMAPS 等，你需要使用第三方 TLS 库进行构建，因为 curl 本身不实现 TLS 协议。

curl 被编写为与大量 TLS 库一起工作：

+   AmiSSL

+   AWS-LC

+   BearSSL

+   BoringSSL

+   GnuTLS

+   libressl

+   mbedTLS

+   OpenSSL

+   rustls

+   Schannel（原生 Windows）

+   Secure Transport（原生 macOS）

+   WolfSSL

当你构建 curl 和 libcurl 以使用这些库之一时，确保你在构建机器上安装了该库及其包含的头文件非常重要。

## configure

下面，你将学习如何告诉 configure 使用不同的库。configure 脚本默认不选择任何 TLS 库。你必须选择一个，或者指示 configure 你想要构建不带 TLS 支持，使用`--without-ssl`。

### OpenSSL, BoringSSL, libressl

```sh
./configure --with-openssl
```

configure 默认在其默认路径中检测到 OpenSSL。你可以选择性地将 configure 指向一个自定义安装路径前缀，以便在其中找到 OpenSSL：

```sh
./configure --with-openssl=/home/user/installed/openssl
```

可选方案 BoringSSL 和 libressl 看起来足够相似，以至于 configure 以与 OpenSSL 相同的方式检测它们。然后它使用额外的措施来确定它使用的是哪种特定风味。

### GnuTLS

```sh
./configure --with-gnutls
```

configure 默认在其默认路径中检测到 GnuTLS。你可以选择性地将 configure 指向一个自定义安装路径前缀，以便在其中找到 gnutls：

```sh
./configure --with-gnutls=/home/user/installed/gnutls
```

### WolfSSL

```sh
./configure --with-wolfssl
```

configure 默认在其默认路径中检测到 WolfSSL。你可以选择性地将 configure 指向一个自定义安装路径前缀，以便在其中找到 WolfSSL：

```sh
./configure --with-wolfssl=/home/user/installed/wolfssl
```

### mbedTLS

```sh
./configure --with-mbedtls
```

configure 默认在其默认路径中检测到 mbedTLS。你可以选择性地将 configure 指向一个自定义安装路径前缀，以便在其中找到 mbedTLS：

```sh
./configure --with-mbedtls=/home/user/installed/mbedtls
```

### Secure Transport

```sh
./configure --with-secure-transport
```

configure 默认在其默认路径中检测到 Secure Transport。你可以选择性地将 configure 指向一个自定义安装路径前缀，以便在其中找到 Secure Transport：

```sh
./configure --with-secure-transport=/home/user/installed/darwinssl
```

### Schannel

```sh
./configure --with-schannel
```

configure 默认在其默认路径中检测到 Schannel。

（WinSSL 曾是 Schannel 的替代名称，而早期 curl 版本则需要`--with-winssl`）

### BearSSL

```sh
./configure --with-bearssl
```

configure 默认在其默认路径中检测到 BearSSL。你可以选择性地将 configure 指向一个自定义安装路径前缀，以便在其中找到 BearSSL：

```sh
./configure --with-bearssl=/home/user/installed/bearssl
```

### Rustls

```sh
./configure --with-rustls
```

当被指示使用 rustls 时，curl 实际上正在尝试查找和使用 rustls-ffi 库——rustls 库的 C API。configure 默认在其默认路径中检测到 rustls-ffi。你可以选择性地将 configure 指向一个自定义安装路径前缀，以便在其中找到 rustls-ffi：

```sh
./configure --with-rustls=/home/user/installed/rustls-ffi
```
