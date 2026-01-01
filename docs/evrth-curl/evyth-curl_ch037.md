# 代码布局

curl 源代码树既不大也不复杂。要记住的关键一点是 libcurl 是库，而这个库是 curl 命令行工具的最大组件。

## root

我们试图将源树根目录中的文件数量保持在最低。如果您检查发布存档与 git 仓库中存储的内容相比，可能会看到文件有细微的差异，因为一些文件是由发布脚本生成的。

其中一些较为显著的包括：

+   `buildconf`：（已弃用）用于从 git 仓库中构建 curl 源代码时构建 configure 和其他内容的脚本。

+   `buildconf.bat`：buildconf 的 Windows 版本。在从 git 检出完整源代码后运行此脚本。

+   `CHANGES`：在发布时生成并放入发布存档。它包含源代码库中的 1000 个最新更改。

+   `configure`：一个生成的脚本，用于在 Unix-like 系统上构建 curl 时生成设置。

+   `COPYING`：详细说明使用代码规则的许可证。

+   `GIT-INFO`：仅在 git 中存在，包含有关从 git 检出代码后如何构建 curl 的信息。

+   `maketgz`：用于生成发布存档和每日快照的脚本

+   `README`：curl 和 libcurl 的简要总结。

+   `RELEASE-NOTES`：包含最新发布版本所做的更改；当在 git 中找到时，包含自上次发布以来旨在进入即将发布版本中的更改。

## lib

此目录包含 libcurl 的完整源代码。它是所有平台的相同源代码——超过一百个 C 源文件和一些额外的私有头文件。用于构建针对 libcurl 的应用程序时使用的头文件不存储在此目录中；请参阅 include/curl。

根据您自己的构建中启用的功能和您的平台提供的功能，某些源文件或源文件的部分可能包含在您的特定构建中未使用的代码。

## lib/vtls

libcurl 中的 VTLS 子部分是所有 libcurl 可以构建以支持的所有 TLS 后端的家园。这个“虚拟”TLS 内部 API 是一个与后端无关的 API，用于内部访问 TLS 和加密函数，而主代码不知道使用了哪个特定的 TLS 库。这允许构建 libcurl 的人从广泛的 TLS 库中选择来构建。

我们还在网站上维护了一个[SSL 比较表](https://curl.se/docs/ssl-compared.html)，以帮助用户。

+   AmiSSL：为 AmigaOS 制作的 OpenSSL 分支（使用`openssl.c`）

+   BearSSL

+   BoringSSL：由 Google 维护的 OpenSSL 分支。（使用`openssl.c`）

+   GnuTLS

+   LibreSSL：由 OpenBSD 团队维护的 OpenSSL 分支。（使用`openssl.c`）

+   mbedTLS

+   OpenSSL

+   rustls：用 Rust 编写的 TLS 库

+   Schannel：Windows 上的原生 TLS 库。

+   安全传输：macOS 上的原生 TLS 库

+   wolfSSL

## src

此目录包含 curl 命令行工具的源代码。这是所有运行此工具的平台相同的源代码。

命令行工具所做的多数工作是将给定的命令行选项转换为相应的 libcurl 选项或选项集，然后确保正确地发出它们，以根据用户的意愿驱动网络传输。

此代码像任何其他应用程序一样使用 libcurl。

## include/curl

这里提供了供 libcurl 使用应用程序使用的公共头文件。其中一些在配置或发布时生成，因此在 git 仓库中看起来与发布存档中的不完全相同。

在现代 libcurl 中，应用程序预期在其 C 源代码中包含的只是`#include <curl/curl.h>`。

## docs

主要的文档位置。此目录中的文本文件通常是纯文本文件。我们已经开始逐渐转向 Markdown 格式，因此一些（但数量正在增加）文件使用`.md`扩展名来表示。

大多数这些文档也会自动显示在 curl 网站上，从文本转换为友好的网页格式/外观。

+   `BINDINGS`: 列出所有已知的 libcurl 语言绑定及其位置

+   `BUGS`: 如何报告错误及其位置

+   `CODE_OF_CONDUCT.md`: 我们期望人们在项目中如何表现

+   `CONTRIBUTE`: 在为项目做出贡献时需要考虑的事项

+   `curl.1`: curl 命令行工具的 man 页面，采用 nroff 格式

+   `curl-config.1`: curl-config 的 man 页面，采用 nroff 格式

+   `FAQ`: 关于各种 curl 相关主题的常见问题

+   `FEATURES`: curl 功能的未完成列表

+   `HISTORY`: 描述项目是如何开始的以及多年来是如何发展的

+   `HTTP2.md`: 如何使用 curl 和 libcurl 进行 HTTP/2

+   `HTTP-COOKIES`: curl 如何支持和使用 HTTP cookies

+   `index.html`: 作为文档索引页面的基本 HTML 页面

+   `INSTALL`: 如何从源代码构建和安装 curl 和 libcurl

+   `INSTALL.cmake`: 如何使用 CMake 构建 curl 和 libcurl

+   `INSTALL.devcpp`: 如何使用 devcpp 构建 curl 和 libcurl

+   `INTERNALS`: curl 和 libcurl 内部结构的详细信息

+   `KNOWN_BUGS`: 已知错误和问题的列表

+   `LICENSE-MIXING`: 描述如何组合不同的第三方模块及其各自的许可证

+   `MAIL-ETIQUETTE`: 在我们的邮件列表上如何进行沟通

+   `MANUAL`: 关于如何使用 curl 的教程式指南

+   `mk-ca-bundle.1`: mk-ca-bundle 工具的 man 页面，采用 nroff 格式

+   `README.cmake`: CMake 详细信息

+   `README.netware`: Netware 详细信息

+   `README.win32`: win32 详细信息

+   `RELEASE-PROCEDURE`: 如何进行 curl 和 libcurl 的发布

+   `RESOURCES`: 进一步阅读 curl 如何做事情的更多资源

+   `ROADMAP.md`: 我们未来想要工作的内容

+   `SECURITY`: 我们如何处理安全漏洞

+   `SSLCERTS`: TLS 证书处理的文档

+   `SSL-PROBLEMS`: 常见 SSL 问题和其原因

+   `THANKS`: 感谢这个广泛的友好人士名单，curl 今天得以存在。

+   `TheArtOfHttpScripting`：使用 curl 进行 HTTP 脚本编写的教程

+   `TODO`：我们可以或你可以着手实现的事情

+   `VERSIONS`：libcurl 版本编号的工作方式

## docs/libcurl

所有 libcurl 函数都有自己的 man 页面，以 .3 扩展名存储在单独的文件中，使用 nroff 格式，位于此目录中。还有一些其他文件，如下所述。

+   `ABI`

+   `index.html`

+   `libcurl.3`

+   `libcurl-easy.3`

+   `libcurl-errors.3`

+   `libcurl.m4`

+   `libcurl-multi.3`

+   `libcurl-share.3`

+   `libcurl-thread.3`

+   `libcurl-tutorial.3`

+   `symbols-in-versions`

## docs/libcurl/opts

此目录包含三个不同 libcurl 函数的单独选项的 man 页面。

`curl_easy_setopt()` 选项以 `CURLOPT_` 开头，`curl_multi_setopt()` 选项以 `CURLMOPT_` 开头，`curl_easy_getinfo()` 选项以 `CURLINFO_` 开头。

## docs/examples

包含大约 100 个独立的示例，旨在帮助读者理解如何使用 libcurl。

参见本书的 libcurl 示例部分。

## scripts

方便的脚本。

+   `contributors.sh`：从给定的哈希/标签以来的 git 仓库中提取所有贡献者。目的是为 RELEASE-NOTES 文件生成一个列表，并允许在更新时手动添加的名称仍然保留在那里。该脚本使用 `THANKS-filter` 文件来重写一些名称。

+   `contrithanks.sh`：从给定的哈希/标签以来的 git 仓库中提取贡献者，过滤掉所有已在 `THANKS` 中提到的名称，然后将 `THANKS` 输出到 stdout，并在末尾附加新贡献者的列表；目的是允许更容易地更新 `THANKS` 文档。该脚本使用 `THANKS-filter` 文件来重写一些名称。

+   `log2changes.pl`：为发布版本生成 `CHANGES` 文件，如发布脚本中使用的那样。它只是简单地转换 git log 输出。

+   `zsh.pl`：为 zsh 壳的用户提供 curl 命令行补全的辅助脚本。
