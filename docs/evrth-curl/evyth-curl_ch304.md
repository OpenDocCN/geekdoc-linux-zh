# 标签

每个测试始终完全在 `<testcase>` 标签内指定。每个测试用例进一步分为四个主要部分：`info`、`reply`、`client` 和 `verify`。

+   **info** 提供有关测试用例的信息

+   **reply** 用于让服务器知道在 curl 发送请求后应该发送什么作为回复

+   **client** 定义了客户端应该如何行为

+   **verify** 定义了如何验证在运行命令后存储的数据是否正确结束

每个主要部分支持一组可用的 *子标签*，可以指定，如果指定，则进行检查/使用。

## `<info>`

### `<keywords>`

一个以换行符分隔的关键字列表，描述了此测试用例使用和测试的内容。尽量使用已经使用过的关键字。这些关键字用于统计/信息目的，以及选择或跳过测试类别。“Keywords” 必须以字母字符、“-”、“[” 或 “{” 开头，并且可以由空格分隔的多个单词组成，这些单词被视为单个标识符。

当使用使用 Hyper 构建的 curl 时，关键字必须包括 HTTP 或 HTTPS 以启动“hyper 模式”并使行结束检查在测试中生效。

## `<reply>`

### `<data [nocheck="yes"] [sendzero="yes"] [base64="yes"] [hex="yes"] [nonewline="yes"]>`

在客户端请求时发送到客户端的数据，并在之后验证其是否安全到达。设置 `nocheck="yes"` 以防止测试脚本验证此数据的到达。

如果数据在起始和结束标签的任何位置包含 `swsclose`，并且这是一个 HTTP 测试，那么在发送此响应后，服务器将关闭连接。如果不是，则连接保持持续。

如果数据在起始和结束标签的任何位置包含 `swsbounce`，HTTP 服务器将检测这是否是使用相同测试和部分号的第二次请求，然后增加部分号一个。这对于身份验证测试和类似测试很有用。

`sendzero=yes` 表示即使数据大小为零字节，(FTP) 服务器也会“发送”数据。用于验证 curl 在零字节传输上的行为。

`base64=yes` 表示测试文件中提供的数据是一块使用 base64 编码的数据。这是测试用例包含二进制数据的唯一方式。（实际上，此属性可以用于任何部分，但对于“data”部分以外的部分来说没有太多意义）。

`hex=yes` 表示数据是一系列十六进制对。它被解码并用作“原始”数据。

`nonewline=yes` 表示在发送或比较数据之前应该从数据中截断最后一个字节（尾随换行符）。

对于 FTP 文件列表，只有当你确保首先将 CWD 做到名为 `test-[number]` 的目录中，其中 `[number]` 是测试用例号时，才使用 `<data>` 部分。否则，FTP 服务器无法知道从哪个测试文件加载列表内容。

### `<dataNUMBER>`

发送此内容而不是 <data> 中的内容。数字 `NUMBER` 由以下设置确定：</data>

+   请求行中的测试数字 >10000，这是 [测试用例编号]%10000 的余数。

+   请求是 HTTP 并包含摘要详情，这会将数字增加 1000

+   如果一个 HTTP 请求是 NTLM 类型-1，则将 1001 添加到数字

+   如果一个 HTTP 请求是 NTLM 类型-3，则将 1002 添加到数字

+   如果一个 HTTP 请求是 Basic 类型且数字已经 >=1000，则增加 1

+   如果一个 HTTP 请求是 Negotiate，则对于具有 Negotiate 授权头且在相同测试用例上的每个请求，数字增加 1

以这种方式动态更改测试数字允许测试工具用于测试认证协商，其中必须发送多个不同的请求才能完成传输。每个请求的响应都位于其自己的数据部分。可以通过指定 `datacheck` 部分来验证整个协商序列。

### `<connect>`

连接部分用于所有 CONNECT 请求而不是“数据”。然后应用数据部分的其余规则，但带有连接前缀。

### `<socks>`

记录的 SOCKS 代理的地址类型和地址详情。

### `<datacheck [mode="text"] [nonewline="yes"]>`

如果数据已发送，但这是之后应该检查的内容。如果设置了 `nonewline=yes`，runtests 在与客户端实际接收到的数据进行比较之前，从数据中截断尾随换行符。

如果在具有文本/二进制差异的平台上的输出是文本模式，请使用 `mode="text"` 属性。

### `<datacheckNUM [nonewline="yes"] [mode="text"]>`

编号 `datacheck` 部分的内文将附加到非编号部分。

### `<size>`

对 ftp SIZE 命令返回的数字（设置为一 1 使此命令失败）

### `<mdtm>`

如果客户端发送了设置为一 1 的 FTP `MDTM` 命令，则返回文件不存在

### `<postcmd>`

特殊用途服务器命令，用于在发送回复后控制其行为。对于 HTTP/HTTPS，这些是支持的：

`wait [secs]` - 暂停给定时间

### `<servercmd>`

服务器特殊命令。

此文件的第一个行总是由测试脚本设置为 `Testnum [number]`，以便服务器读取以了解客户端即将发起的测试。

#### 对于 FTP/SMTP/POP/IMAP

+   `REPLY [command] [return value] [response string]` - 改变服务器对 [command] 的响应方式。[response string] 被评估为 perl 字符串，因此它可以包含嵌入的 `\r\n`，例如。有一个特殊的 [command] 名为“welcome”（不带引号），这是连接时立即发送的欢迎字符串。

+   `REPLYLF`（类似于上面，但只发送以 LF 结尾的响应，而不是 CRLF）

+   `COUNT [command] [number]` - 仅对 `[command]` 进行 `REPLY` 更改 `[number]` 次，然后返回到内置方法

+   `DELAY [command] [secs]` - 延迟响应此命令给定时间

+   `RETRWEIRDO` - 启用在文件传输时一次性出现多个响应行时的“怪异”RETR 情况

+   `RETRNOSIZE` - 确保 RETR 响应不包含文件大小

+   `NOSAVE` - 不要保存接收到的数据

+   `SLOWDOWN` - 使用 0.01 秒的延迟发送 FTP 响应，每个字节之间延迟

+   `PASVBADIP` - 使 PASV 在其 227 响应中发送非法 IP

+   `CAPA [能力]` - 启用对能力和指定用于 IMAP `CAPABILITY`、POP3 `CAPA` 和 SMTP `EHLO` 命令的空格分隔的能力列表的支持

+   `AUTH [机制]` - 启用对 SASL 身份验证的支持并指定用于 IMAP、POP3 和 SMTP 的空格分隔的机制列表

+   `STOR [消息]` 在 `STOR` 后用此消息作为默认响应

#### 对于 HTTP/HTTPS

+   `auth_required` 如果此选项已设置且未进行身份验证就发送了 POST/PUT 请求，则服务器不会等待发送完整的请求体

+   `idle` - 接收到请求后什么也不做，只是“空闲”

+   `stream` - 连续向客户端发送数据，永不结束

+   `writedelay: [毫秒]` 在回复包之间延迟指定的时间

+   `skip: [数字]` - 指示服务器忽略从 PUT 或 POST 请求中读取这么多字节

+   `rtp: part [num] channel [num] size [num]` - 在选择的通道上以指定的有效载荷大小流式传输给定部分的伪造 RTP 数据包

+   `connection-monitor` - 当使用时，在连接断开时将 `[DISCONNECT]` 记录到 `server.input` 日志中。

+   `upgrade` - 当找到 HTTP 升级标头时，服务器将升级到 http2

+   `swsclose` - 指示服务器在响应后关闭连接

+   `no-expect` - 如果存在 Expect:，则不读取请求体

#### 对于 TFTP

`writedelay: [秒]` 在回复包之间延迟指定的时间（每个数据包为 512 字节有效载荷）

## `<client>`

### `<server>`

本测试用例需要/使用的服务器。可用服务器：

+   `file`

+   `ftp-ipv6`

+   `ftp`

+   `ftps`

+   `gopher`

+   `gophers`

+   `http-ipv6`

+   `http-proxy`

+   `http-unix`

+   `http/2`

+   `http`

+   `https`

+   `httptls+srp-ipv6`

+   `httptls+srp`

+   `imap`

+   `mqtt`

+   `none`

+   `pop3`

+   `rtsp-ipv6`

+   `rtsp`

+   `scp`

+   `sftp`

+   `smtp`

+   `socks4`

+   `socks5`

每行只能输入一个服务器。本小节是必需的。

### `<features>`

必须在客户端/库中存在的功能列表，以便本测试能够运行。如果缺少必需的功能，则测试将被跳过。

或者，可以通过在功能前加上感叹号来表示该功能不是必需的。如果该功能存在，则测试将被跳过。

在此处可测试的功能：

+   `alt-svc`

+   `bearssl`

+   `c-ares`

+   `cookies`

+   `crypto`

+   `debug`

+   `DoH`

+   `getrlimit`

+   `GnuTLS`

+   `GSS-API`

+   `h2c`

+   `HSTS`

+   `HTTP-auth`

+   `http/2`

+   `hyper`

+   `idn`

+   `ipv6`

+   `Kerberos`

+   `large_file`

+   `ld_preload`

+   `libssh2`

+   `libssh`

+   `oldlibssh`（版本在 0.9.4 之前）

+   `libz`

+   `manual`

+   `Mime`

+   `netrc`

+   `NTLM`

+   `OpenSSL`

+   `parsedate`

+   `proxy`

+   `PSL`

+   `rustls`

+   `Schannel`

+   `sectransp`

+   `shuffle-dns`

+   `socks`

+   `SPNEGO`

+   `SSL`

+   `SSLpinning`

+   `SSPI`

+   `threaded-resolver`

+   `TLS-SRP`

+   `TrackMemory`

+   `typecheck`

+   `Unicode`

+   `unittest`

+   `unix-sockets`

+   `verbose-strings`

+   `wakeup`

+   `win32`

+   `wolfssh`

+   `wolfssl`

除了 curl 支持的所有协议外。只有当协议与服务器不同时才需要指定协议（当服务器是`none`时很有用）。

### `<killserver>`

使用与`<server>`中相同的语法，但在此处提到这些服务器在测试用例完成后会被明确杀死。只有在没有其他替代方案时才使用此选项。当然，使用此选项需要后续测试重新启动服务器。

### `<precheck>`

在测试之前由测试脚本运行的命令行。如果命令显示输出或返回代码非零，则测试将被跳过，并且（单行）输出将显示为不运行测试的原因。

### `<postcheck>`

在测试之后由测试脚本运行的命令行。如果命令以非零状态码退出，则测试被认为是失败的。

### `<tool>`

调用而不是“curl”的工具名称。此工具必须在`libtest/`目录中构建并存在（如果工具名称以`lib`开头）或`unit/`目录中（如果工具名称以`unit`开头）。

### `<name>`

简短的测试用例描述，在测试运行时显示。

### `<setenv>`

```sh
variable1=contents1
variable2=contents2
```

在实际命令运行之前将指定的环境变量设置为指定的值。命令运行后，它们会被清除。

### `<command [option="no-output/no-include/force-output/binary-trace"] [timeout="secs"][delay="secs"][type="perl/shell"]>`

要运行的命令行。

注意，传递给服务器的 URL 实际上控制着返回的数据。URL 中的最后一个斜杠后面必须跟一个数字。该数字（N）由测试服务器用于加载测试用例 N 并返回`<reply><data></data></reply>`部分中定义的数据。

如果上面没有找到测试编号，HTTP 测试服务器使用给定主机名中最后一个点的后面的数字（这样即使测试编号仍然可以通过 CONNECT 传递）来处理“foo.bar.123”作为测试用例 123。或者，如果提供了 IPv6 地址给 CONNECT，则地址中的最后一个十六进制组用作测试编号。例如，地址“[1234::ff]”将被处理为测试用例 255。

将`type="perl"`设置为将测试用例编写为 perl 脚本。这意味着没有内存调试，并且 valgrind 在此测试中会被关闭。

将`type="shell"`设置为将测试用例编写为 shell 脚本。这意味着没有内存调试，并且 valgrind 在此测试中会被关闭。

将`option="no-output"`设置为防止测试脚本在`--output`参数上附加，该参数将输出重定向到文件。如果使用 verify/stdout 部分，则也不会添加`--output`。

将`option="force-output"`设置为即使在测试其他情况下写入 verify stdout 时也要使用`--output`。

将`option="no-include"`设置为防止测试脚本在`--include`参数上附加。

将`option="binary-trace"`设置为使用`--trace`而不是`--trace-ascii`进行跟踪。适用于面向二进制的协议，如 MQTT。

将`timeout="secs"`设置为覆盖默认服务器日志顾问读取锁的超时时间。此超时由测试工具使用，一旦命令执行完成，将等待测试服务器写入服务器端日志文件并移除建议不要读取的锁。参数“secs”是超时的非负整数秒数。此`timeout`属性出于完整性考虑而记录，但这是深度测试工具的内容，并且仅适用于特定测试用例。请避免使用它。

将`delay="secs"`设置为在命令执行完成后和`<postcheck>`部分运行之前引入时间延迟。参数“secs”是延迟的非负整数秒数。此‘delay’属性旨在用于特定测试用例，通常不需要。

### `<file name="log/filename" [nonewline="yes"]>`

在运行测试用例之前，使用此内容创建命名文件，这对于测试用例需要操作的文件很有用。

如果使用`nonewline="yes"`，则创建的文件将移除最后的换行符。

### `<stdin [nonewline="yes"]>`

将此给定数据通过 stdin 传递给工具。

如果设置`nonewline`，我们在将其与客户端实际接收到的数据进行比较之前，将此给定数据的尾部换行符截断。

## `<verify>`

### `<errorcode>`

cURL 应返回的数值错误代码。通过逗号分隔多个数字来指定接受的错误代码列表。请参阅测试 237 以获取示例。

### `<strip>`

每行一个正则表达式，在比较之前从协议转储中移除。这对于移除对动态变化的协议数据（如端口号或用户代理字符串）的依赖很有用。

### `<strippart>`

每行一个操作，该操作在协议转储上执行。这是相当高级的。例如：`s/^EPRT .*/EPRT stripped/`。

### `<protocol [nonewline="yes"]>`

当设置`nonewline`时，如果 cURL 应传输协议转储，我们在将其与客户端实际发送的数据进行比较之前，将此给定数据的尾部换行符截断。在比较之前应用`<strip>`和`<strippart>`规则。

### `<proxy [nonewline="yes"]>`

当使用 HTTP 代理服务器时，如果设置`nonewline`，我们在将其与客户端实际发送的数据进行比较之前，将此给定数据的尾部换行符截断。在比较之前应用`<strip>`和`<strippart>`规则。

### `<stderr [mode="text"] [nonewline="yes"]>`

这验证了这些数据已传递到 stderr。

如果在具有文本/二进制差异的平台上的输出是文本模式，请使用`mode="text"`属性。

如果设置`nonewline`，我们在将其与客户端实际接收到的数据进行比较之前，将此给定数据的尾部换行符截断。

### `<stdout [mode="text"] [nonewline="yes"]>`

这验证了这些数据已传递到 stdout。

如果在具有文本/二进制差异的平台上的输出是文本模式，请使用`mode="text"`属性。

如果设置了 `nonewline`，我们在与客户端实际收到的数据进行比较之前，将此给定数据的尾部换行符截断

### `<file name="log/filename" [mode="text"]>`

测试完成后，文件内容必须与此完全相同。如果输出在具有文本/二进制差异的平台上的文本模式下，请使用 `mode="text"` 属性。

### `<file1>`

可以将 1 到 4 添加到 'file' 以比较更多文件。

### `<file2>`

### `<file3>`

### `<file4>`

### `<stripfile>`

每行一个 Perl 操作符，该操作符在比较测试文件中存储的内容之前，对输出文件或 stdout 进行操作。这相当高级。例如："s/^EPRT .*/EPRT stripped/"

### `<stripfile1>`

可以将 1 到 4 添加到 `stripfile` 以剥离相应的 <filen>content</filen>

### `<stripfile2>`

### `<stripfile3>`

### `<stripfile4>`

### `<upload>`

上传数据 curl 应发送的内容

### `<valgrind>`

disable - 禁用此测试的 valgrind 日志检查
