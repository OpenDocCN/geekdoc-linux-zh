# SSH 密钥

此回调函数通过`CURLOPT_SSH_KEYFUNCTION`设置。

当`known_host`匹配完成时会被调用，以便应用程序可以行动并决定 libcurl 如何继续。如果`CURLOPT_SSH_KNOWNHOSTS`也被设置，则会调用回调函数。

回调函数的参数是旧密钥和新密钥，并且期望回调函数返回一个返回码，告诉 libcurl 如何行动：

`CURLKHSTAT_FINE_REPLACE` - 新的主机+密钥被接受，libcurl 在继续连接之前将旧的主机+密钥替换到 known_hosts 文件中。如果新主机+密钥组合尚未存在于内存中的 known_host 池中，它也会添加到该池中。向文件添加数据是通过完全用新副本替换文件来完成的，因此文件权限必须允许这样做。

`CURLKHSTAT_FINE_ADD_TO_FILE` - 主机+密钥被接受，libcurl 在继续连接之前将其追加到 known_hosts 文件中。如果主机+密钥组合尚未存在于内存中的 known_host 池中，它也会添加到该池中。向文件添加数据是通过完全用新副本替换文件来完成的，因此文件权限必须允许这样做。

`CURLKHSTAT_FINE` - 主机+密钥被接受，libcurl 继续连接。如果主机+密钥组合尚未存在于内存中的 known_host 池中，它也会添加到该池中。

`CURLKHSTAT_REJECT` - 主机+密钥被拒绝。libcurl 拒绝连接继续并关闭。

`CURLKHSTAT_DEFER` - 主机+密钥被拒绝，但请求保持 SSH 连接活跃。当应用程序想要以某种方式返回并处理主机+密钥情况，然后重试而不需要从头设置开销时，可以使用此功能。
