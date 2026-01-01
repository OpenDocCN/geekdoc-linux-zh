# SSL 上下文

libcurl 提供了一个特殊的 TLS 相关回调，称为`CURLOPT_SSL_CTX_FUNCTION`。此选项仅适用于由 OpenSSL、wolfSSL 或 mbedTLS 提供的 libcurl，如果 libcurl 是用其他 TLS 后端构建的，则此选项不起作用。

此回调在 libcurl 处理完所有其他 TLS 相关选项之后，在初始化 TLS 连接之前被调用，以便给应用程序最后一次修改 TLS 初始化行为的机会。传递给回调的第二个参数中的`ssl_ctx 参数`实际上是 OpenSSL 或 wolfSSL 的 SSL 库的`SSL_CTX`的指针，以及 mbedTLS 的`mbedtls_ssl_config`的指针。如果回调返回错误，则不会尝试建立连接，并且操作返回回调的错误代码。使用`CURLOPT_SSL_CTX_DATA`选项设置`userptr`参数。

此函数在服务器上建立的所有新连接期间被调用，在 TLS 协商过程中。每次 TLS 上下文都指向一个新初始化的对象。
