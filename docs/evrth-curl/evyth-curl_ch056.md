# 方案

URL 以“方案”开头，这是`http://`部分的官方名称。这表明 URL 使用哪种协议。方案必须是 curl 支持的可识别方案，否则它会显示错误消息并停止。此外，方案不能以空格开头或包含任何空格。

## 方案分隔符

方案标识符通过`://`序列与 URL 的其余部分分开。这是一个冒号和两个正斜杠。存在只有单个斜杠的 URL 格式，但 curl 不支持任何这些格式。有两个额外的注意事项需要注意，关于斜杠的数量：

curl 允许一些非法语法，并尝试在内部进行纠正；因此，它也理解并接受一个或三个斜杠的 URL，尽管实际上它们并不是正确形成的 URL。curl 这样做是因为浏览器开始采用这种做法，因此偶尔会在野外使用这样的 URL。

`file://` URL 的格式为`file://<hostname>/<path>`，但可以使用的主机名只有`localhost`、`127.0.0.1`或空白（什么都没有）：

```sh
file://localhost/path/to/file
file://127.0.0.1/path/to/file
file:///path/to/file
```

在其中插入任何其他主机名会使 curl 的最新版本返回错误。

特别注意上面的第三个例子（`file:///path/to/file`）。路径前有**三个**斜杠。这又是一个常见的错误区域，浏览器允许用户使用错误的语法，因此作为一个特殊例外，Windows 上的 curl 也允许这种不正确的格式：

```sh
file://X:/path/to/file
```

…其中 X 是 Windows 风格的驱动器字母。

## 没有方案

作为便利，curl 还允许用户从 URL 中省略方案部分。然后它根据主机名的前一部分猜测要使用的协议。这种猜测是基本的，因为它只是检查主机名的前一部分是否与一组协议之一匹配，并假设您打算使用该协议。这种启发式方法基于服务器传统上被这样命名的这一事实。通过这种方式检测到的协议是 FTP、DICT、LDAP、IMAP、SMTP 和 POP3。在无方案的 URL 中，任何其他主机名都会使 curl 默认为 HTTP。

例如，这可以从 FTP 站点获取一个文件：

```sh
curl ftp.funet.fi/README
```

当从 HTTP 服务器获取数据时：

```sh
curl example.com
```

您可以使用`--proto-default`选项将默认协议修改为 HTTP 以外的其他协议。

## 支持的方案

curl 支持或可以构建以支持以下传输方案和协议：

DICT, FILE, FTP, FTPS, GOPHER, GOPHERS, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, MQTT, POP3, POP3S, RTMP, RTMPS, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET, TFTP, WS 和 WSS
