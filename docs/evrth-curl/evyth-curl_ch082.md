# 写出

`--write-out`或简写为`-w`，在传输完成后输出文本和信息。它提供了一系列变量，您可以将这些变量包含在输出中，这些变量包含已设置的值和传输的信息。

通过将纯文本传递给此选项来指示 curl 输出一个字符串：

```sh
curl -w "formatted string" http://example.com/
```

…你也可以让 curl 从指定的文件中读取该字符串，如果在该字符串前加上‘@’：

```sh
curl -w @filename http://example.com/
```

…或者甚至让 curl 从 stdin 读取字符串，如果您使用`-`作为文件名：

```sh
curl -w @- http://example.com/
```

## 变量

可用的变量通过在字符串中写入`%{variable_name}`来访问，该变量将被正确的值替换。要输出一个普通的`%`，请将其写作`%%`。您还可以使用`\n`输出换行符，使用`\r`输出回车符，使用`\t`输出制表符。

例如，我们可以这样输出 HTTP 传输的 Content-Type 和响应代码，用换行符和一些额外的文本分隔：

```sh
curl -w "Type: %{content_type}\nCode: %{response_code}\n" \
  http://example.com
```

默认情况下，输出发送到 stdout，所以您可能想确保您没有也将下载的内容发送到 stdout，否则您可能很难分离数据；或者使用`%{stderr}`将输出发送到 stderr。

## HTTP 头部

此选项还提供了一个简单易用的方法来输出最近一次传输的 HTTP 响应头内容。

在字符串中使用`%header{name}`，其中`name`是头部的无大小写名称（不带尾随冒号）。然后输出头部内容将精确地显示为通过网络发送的内容，前后空白被删除。如下所示：

```sh
curl -w "Server: %header{server}\n" http://example.com
```

## 输出

默认情况下，此选项使选定的数据输出到 stdout。如果这还不够好，可以使用伪变量`%{stderr}`将（以下）部分重定向到 stderr，而`%{stdout}`则将其带回 stdout。

从 curl 8.3.0 版本开始，有一个功能允许用户将 write-out 输出发送到文件：`%output{filename}`。然后数据将被写入该文件。如果您希望 curl 向该文件追加而不是从头创建它，请将文件名前缀为`>>`。如下所示：`%output{>>filename}`。

写出参数可以包括用户认为合适的 stderr、stdout 和文件输出。

## Windows

**注意：** 在 Windows 中，`%`符号是一个特殊符号，用于展开环境变量。在批处理文件中，使用此选项时，所有`%`的出现都必须加倍，以正确转义。如果在命令提示符中使用此选项，则`%`无法转义，可能会发生意外的展开。

## 可用的–write-out 变量

这些变量中的一些在非常旧的 curl 版本中不可用。

| 变量 | 描述 |
| --- | --- |
| `certs` | 输出最近一次 TLS 握手的证书链 - 包括详细信息。（自 7.88.0 版本引入） |
| `content_type` | 请求文档的 Content-Type，如果有。 |
| `errormsg` | 传输错误信息。如果没有错误发生，则为空。（自 7.75.0 版本引入） |
| `exitcode` | 传输的数值退出代码。如果没有发生错误，则为 0。（自 7.75.0 版本引入） |
| `filename_effective` | curl 写入的最终文件名。如果 curl 被指示使用`--remote-name`或`--output`选项写入文件，则非常有用。它与`--remote-header-name`选项结合使用时最有用。 |
| `ftp_entry_path` | 当登录到远程 FTP 服务器时 curl 最终到达的初始路径。 |
| `http_code` | 现在被称为`response_code`的前一个变量名。 |
| `http_connect` | 在 curl CONNECT 请求的最后响应（来自代理）中找到的数值代码。 |
| `http_version` | 使用的 HTTP 版本。 |
| `json` | 将所有写入变量作为一个单独的 JSON 对象输出。（自 7.72.0 版本引入） |
| `local_ip` | 最近使用连接的本地端 IP 地址 - 可以是 IPv4 或 IPv6 |
| `local_port` | 最近使用连接的本地端口号 |
| `method` | 最近请求使用的 HTTP 方法。（自 7.72.0 版本引入） |
| `num_certs` | 最近 TLS 握手中证书的数量。（自 7.88.0 版本引入） |
| `num_connects` | 在最近传输中建立的新连接数量。 |
| `num_headers` | 最后响应中的响应头数量 |
| `num_redirects` | 请求中跟随的重定向数量。 |
| `onerror` | 如果传输因错误而结束，则显示字符串的其余部分，否则在这里停止。（自 7.75.0 版本引入） |
| `proxy_ssl_verify_result` | 与代理通信时请求的 SSL 对等证书验证的结果。0 表示验证成功。 |
| `redirect_url` | 当进行 HTTP 请求时，如果没有使用 `-L` 来跟随重定向，将实际重定向到的 URL。 |
| `remote_ip` | 最近使用连接的远程 IP 地址 — 可以是 IPv4 或 IPv6。 |
| `remote_port` | 最近建立的连接的远程端口号。 |
| `response_code` | 在最后传输中找到的数值响应代码。 |
| `scheme` | 在上一个 URL 中使用的方案 |
| `size_download` | 下载的总字节数。 |
| `size_header` | 下载的头的总字节数。 |
| `size_request` | 在 HTTP 请求中发送的总字节数。 |
| `size_upload` | 上传的总字节数。 |
| `speed_download` | curl 测量的完整下载的平均下载速度，以字节每秒为单位。 |
| `speed_upload` | curl 测量的完整上传的平均上传速度，以字节每秒为单位。 |
| `ssl_verify_result` | 请求的 SSL 对等证书验证的结果。0 表示验证成功。 |
| `stderr` | 使其余输出写入 stderr。 |
| `stdout` | 使其余输出写入 stdout。 |
| `time_appconnect` | 从开始到与远程主机进行 SSL/SSH 等连接/握手完成所需的时间（以秒为单位）。 |
| `time_connect` | 从开始到与远程主机（或代理）的 TCP 连接完成所需的时间（以秒为单位）。 |
| `time_namelookup` | 从开始到域名解析完成所需的时间（以秒为单位）。 |
| `time_pretransfer` | 从开始到文件传输即将开始所需的时间（以秒为单位）。这包括所有与特定协议（s）相关的预传输命令和协商。 |
| `time_redirect` | 包括名称查找、连接、预传输和传输的所有重定向步骤所需的时间（以秒为单位），直到开始最终事务。time_redirect 是多次重定向的完整执行时间。 |
| `time_starttransfer` | 从开始到第一个字节即将被传输所需的时间（以秒为单位）。这包括 time_pretransfer 以及服务器计算结果所需的时间。 |
| `time_total` | 全部操作持续的总时间（以秒为单位）。时间以毫秒分辨率显示。 |
| `url` | 传输中使用的 URL。（自 7.75.0 版本引入） |
| `url_effective` | 最后被获取的 URL。如果你告诉 curl 跟随 Location:标题（使用`-L`），这尤其有意义。 |
| `urlnum` | 传输中使用的 URL 的基于 0 的数值索引。（自 7.75.0 版本引入） |

在 curl 8.1.0 版本中，添加了仅输出特定 URL 组件的变量，当`url`或`url_effective`变量显示的内容超过所需时使用。

| 变量 | 描述 |
| --- | --- |
| `url.scheme` | 被获取的 URL 的方案部分。 |
| `url.user` | 被获取的 URL 的用户部分。 |
| `url.password` | 被获取的 URL 的密码部分。 |
| `url.options` | 被获取的 URL 的选项部分。仅适用于某些方案。 |
| `url.host` | 被获取的 URL 的主机名部分。 |
| `url.path` | 被获取的 URL 的路径部分。 |
| `url.query` | 被获取的 URL 的查询部分。 |
| `url.fragment` | 被获取的 URL 的片段部分。 |
| `url.zoneid` | 被获取的 URL 的域 ID 部分。仅当主机名是 IPv6 地址时才可用。 |
| `urle.scheme` | 被获取的有效（最后）URL 的方案部分。 |
| `urle.user` | 被获取的有效（最后）URL 的用户部分。 |
| `urle.password` | 被获取的有效（最后）URL 的密码部分。 |
| `urle.options` | 被获取的有效（最后）URL 的选项部分。仅适用于某些方案。 |
| `urle.host` | 被获取的有效（最后）URL 的主机名部分。 |
| `urle.path` | 被获取的有效（最后）URL 的路径部分。 |
| `urle.query` | 被获取的有效（最后）URL 的查询部分。 |
| `urle.fragment` | 被获取的有效（最后）URL 的片段部分。 |
| `urle.zoneid` | 被获取的有效（最后）URL 的区域 ID 部分。仅在主机名为 IPv6 地址时可用。 |
