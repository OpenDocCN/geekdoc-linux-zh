# 测试文件格式

每个 curl 测试都使用单个文本文件设计，格式类似于 XML。

标签标记了所有部分的开始和结束，并且每个标签必须单独一行。注释可以是 XML 风格（用 `<!--` 和 `-->` 包围）或 shell 脚本风格（以 `#` 开头）并且必须单独一行，不能与实际测试数据并列。大多数测试数据文件是语法有效的 XML，尽管一些文件不是（不支持字符实体和保留行尾的回车和换行符是最大的区别）。

所有测试都必须以 `<testcase>` 标签开始，它包含文件的其余部分。下面将介绍其他标签。

每个测试文件都命名为 `tests/data/testNUMBER`，其中 `NUMBER` 是唯一的数值测试标识符。每个测试都必须使用自己的专用编号。这个编号除了标识测试外，没有其他意义。

测试文件定义了确切要运行的命令行或工具、要调用的测试服务器以及它们应该如何响应、应该发生的确切协议交换、预期的输出和返回代码以及更多内容。

当设置名称时，所有内容都写入它们各自的标签中，如下所示：

```sh
<name>
HTTP with host name written backwards
</name>
```

## 关键字

每个测试在文件顶部设置一个或多个 `<keywords>`。它们被用作“标签”，以标识由此测试用例测试的功能和协议。`runtests.pl` 可以被配置为仅运行匹配（或不匹配）此类关键字的测试。

## 预处理

在启动时，每个测试输入文件都由 `runtests.pl` 进行预处理。这意味着变量、宏和关键字会被展开，并在 `tests/log/testNUMBER` 中存储临时文件版本 - 然后，这个文件被所有测试服务器等使用。

这种处理允许测试格式提供像 `%repeat` 这样的功能，以创建非常大的测试文件，而不会相应地膨胀输入文件。

## Base64 编码

在预处理阶段，可以使用特殊指令让 runtests.pl 对某个部分进行 base64 编码，并将其插入到生成的输出文件中。这对于测试用例特别有用，因为测试工具预期会传递 base64 编码的内容，这些内容可能使用了特定于此次测试调用的动态信息，例如服务器端口号。

要将 base64 编码的字符串插入到输出中，使用以下语法：

```sh
%b64[ data to encode ]b64%
```

要编码的数据可以使用以下提到的任何现有变量，甚至可以插入单独的字节。例如，插入 HTTP 服务器的端口号（ASCII 编码）后跟一个空格和十六进制字节 9a：

```sh
%b64[%HTTPPORT %9a]b64%
```

## 十六进制解码

在预处理阶段，可以使用特殊指令让 runtests.pl 生成一系列二进制字节。

要从十六进制编码的字符串中插入一系列字节，使用以下语法：

```sh
%hex[ %XX-encoded data to decode ]hex%
```

例如，要将二进制八位字节 0、1 和 255 插入到测试文件中：

```sh
%hex[ %00%01%FF ]hex%
```

## 重复内容

在预处理阶段，可以使用特殊指令让 runtests.pl 生成重复的字节序列。

要插入重复的字节序列，使用此语法使`<string>`重复`<number>`次。数字必须是 1 或更大，字符串可以包含`%HH`十六进制代码：

```sh
%repeat[<number> x <string>]%
```

例如，要插入单词“hello” 100 次：

```sh
%repeat[100 x hello]%
```

## 条件行

测试文件中的行可以根据特定特征（见下文“特征”部分）是否设置而条件性地显示。如果存在特定特征，则输出以下行，否则不输出任何内容，直到出现后续的`else`或`endif`子句。如下所示：

```sh
%if brotli
Accept-Encoding
%endif
```

它还可以检查逆条件，所以如果特征**没有**通过感叹号设置：

```sh
%if !brotli
Accept-Encoding: not-brotli
%endif
```

您还可以使用“else”子句来获取相反条件的输出，例如：

```sh
%if brotli
Accept-Encoding: brotli
%else
Accept-Encoding: nothing
%endif
```

**注意**，不能有嵌套条件。您一次只能执行一个条件，并且只能检查一个特征。

## 变量

在测试预处理时，测试文件中的一系列变量会被替换为当时的内容。

可用的替换变量包括：

+   `%CLIENT6IP` - 运行 curl 的客户端的 IPv6 地址

+   `%CLIENTIP` - 运行 curl 的客户端的 IPv4 地址

+   `%CURL` - curl 可执行文件的路径

+   `%FILE_PWD` - 当前目录，在 Windows 中前面带有斜杠

+   `%FTP6PORT` - FTP 服务器的 IPv6 端口号

+   `%FTPPORT` - FTP 服务器的端口号

+   `%FTPSPORT` - FTPS 服务器的端口号

+   `%FTPTIME2` - 应该刚好足够从测试 FTP 服务器接收响应的超时时间（秒）

+   `%FTPTIME3` - 比 `%FTPTIME2` 更长

+   `%GOPHER6PORT` - Gopher 服务器的 IPv6 端口号

+   `%GOPHERPORT` - Gopher 服务器的端口号

+   `%GOPHERSPORT` - Gophers 服务器的端口号

+   `%HOST6IP` - 运行此测试的主机的 IPv6 地址

+   `%HOSTIP` - 运行此测试的主机的 IPv4 地址

+   `%HTTP6PORT` - HTTP 服务器的 IPv6 端口号

+   `%HTTPPORT` - HTTP 服务器的端口号

+   `%HTTP2PORT` - HTTP/2 服务器的端口号

+   `%HTTPSPORT` - HTTPS 服务器的端口号

+   `%HTTPSPROXYPORT` - HTTPS 代理的端口号

+   `%HTTPTLS6PORT` - HTTP TLS 服务器的 IPv6 端口号

+   `%HTTPTLSPORT` - HTTP TLS 服务器的端口号

+   `%HTTPUNIXPATH` - HTTP 服务器的 Unix 套接字路径

+   `%SOCKSUNIXPATH` - SOCKS 服务器的 Unix 套接字的绝对路径

+   `%IMAP6PORT` - IMAP 服务器的 IPv6 端口号

+   `%IMAPPORT` - IMAP 服务器的端口号

+   `%MQTTPORT` - MQTT 服务器的端口号

+   `%TELNETPORT` - telnet 服务器的端口号

+   `%NOLISTENPORT` - 没有服务监听的端口号

+   `%POP36PORT` - POP3 服务器的 IPv6 端口号

+   `%POP3PORT` - POP3 服务器的端口号

+   `%POSIX_PWD` - 对 mingw 友好的当前目录

+   `%PROXYPORT` - HTTP 代理的端口号

+   `%PWD` - 当前目录

+   `%RTSP6PORT` - RTSP 服务器的 IPv6 端口号

+   `%RTSPPORT` - RTSP 服务器的端口号

+   `%SMBPORT` - SMB 服务器的端口号

+   `%SMBSPORT` - SMBS 服务器的端口号

+   `%SMTP6PORT` - SMTP 服务器的 IPv6 端口号

+   `%SMTPPORT` - SMTP 服务器的端口号

+   `%SOCKSPORT` - SOCKS4/5 服务器的端口号

+   `%SRCDIR` - 源目录的完整路径

+   `%SSHPORT` - SCP/SFTP 服务器的端口号

+   `%SSHSRVMD5` - SSH 服务器的公钥的 MD5

+   `%SSHSRVSHA256` - SSH 服务器的公钥的 SHA256

+   `%SSH_PWD` - 对 SSH 服务器友好的当前目录

+   `%TESTNUMBER` - 测试用例的编号

+   `%TFTP6PORT` - TFTP 服务器的 IPv6 端口号

+   `%TFTPPORT` - TFTP 服务器的端口号

+   `%USER` - 运行测试的用户登录 ID

+   `%VERSION` - 被测试 curl 的完整版本号
