# 跟踪选项

有时候`-v`是不够的。特别是当你想要存储包括实际传输数据的完整流时。

对于 curl 使用 HTTPS、FTPS 或 SFTP 等协议进行加密文件传输的情况，其他网络监控工具（如 Wireshark 或 tcpdump）无法像 curl 那样轻松地完成这项工作。

对于这一点，curl 提供了另外两个选项，你可以用它们来代替`-v`。

`--trace [filename]`将完整的跟踪保存到指定的文件名中。你也可以使用`-`（单个减号）来代替文件名，以便将其传递到标准输出。你会这样使用它：

```sh
$ curl --trace dump http://example.com
```

完成后，会有一个`dump`文件，可能会相当大。在这种情况下，`dump`文件的前 15 行看起来像：

```sh
== Info: Rebuilt URL to: http://example.com/
== Info:   Trying 93.184.216.34...
== Info: Connected to example.com (93.184.216.34) port 80 (#0)
=> Send header, 75 bytes (0x4b)
0000: 47 45 54 20 2f 20 48 54 54 50 2f 31 2e 31 0d 0a GET / HTTP/1.1..
0010: 48 6f 73 74 3a 20 65 78 61 6d 70 6c 65 2e 63 6f Host: example.co
0020: 6d 0d 0a 55 73 65 72 2d 41 67 65 6e 74 3a 20 63 m..User-Agent: c
0030: 75 72 6c 2f 37 2e 34 35 2e 30 0d 0a 41 63 63 65 url/7.45.0..Acce
0040: 70 74 3a 20 2a 2f 2a 0d 0a 0d 0a                pt: */*....
<= Recv header, 17 bytes (0x11)
0000: 48 54 54 50 2f 31 2e 31 20 32 30 30 20 4f 4b 0d HTTP/1.1 200 OK.
0010: 0a                                              .
<= Recv header, 22 bytes (0x16)
0000: 41 63 63 65 70 74 2d 52 61 6e 67 65 73 3a 20 62 Accept-Ranges: b
0010: 79 74 65 73 0d 0a                               ytes..
```

每一个发送和接收的字节都会单独以十六进制数字显示。接收到的头部信息按行输出。

如果你认为十六进制数没有帮助，你可以尝试使用`--trace-ascii [filename]`，同时它也接受`-`作为标准输出，这使得前 15 行跟踪看起来像：

```sh
== Info: Rebuilt URL to: http://example.com/
== Info:   Trying 93.184.216.34...
== Info: Connected to example.com (93.184.216.34) port 80 (#0)
=> Send header, 75 bytes (0x4b)
0000: GET / HTTP/1.1
0010: Host: example.com
0023: User-Agent: curl/7.45.0
003c: Accept: */*
0049:
<= Recv header, 17 bytes (0x11)
0000: HTTP/1.1 200 OK
<= Recv header, 22 bytes (0x16)
0000: Accept-Ranges: bytes
<= Recv header, 31 bytes (0x1f)
0000: Cache-Control: max-age=604800
```

## 时间戳

`--trace-time`选项在打印每一行时，在详细/跟踪输出前加上高分辨率计时器。它与常规的`-v / --verbose`选项以及`--trace`和`--trace-ascii`一起工作。

一个例子可能看起来像这样：

```sh
$ curl -v --trace-time http://example.com
23:38:56.837164 * Rebuilt URL to: http://example.com/
23:38:56.841456 *   Trying 93.184.216.34...
23:38:56.935155 * Connected to example.com (93.184.216.34) port 80 (#0)
23:38:56.935296 > GET / HTTP/1.1
23:38:56.935296 > Host: example.com
23:38:56.935296 > User-Agent: curl/7.45.0
23:38:56.935296 > Accept: */*
23:38:56.935296 >
23:38:57.029570 < HTTP/1.1 200 OK
23:38:57.029699 < Accept-Ranges: bytes
23:38:57.029803 < Cache-Control: max-age=604800
23:38:57.029903 < Content-Type: text/html
---- snip ----
```

行都是本地时间，以小时:分钟:秒表示，然后是那一秒中的微秒数。

## 识别传输和连接

虽然使用这些选项在屏幕或文件上显示的跟踪信息流是连续的，即使你的命令行可能使 curl 使用大量的单独连接和不同的传输，但有时你想要看到以下各种信息对应的具体传输或连接。为了更好地理解跟踪输出。

你可以然后添加`--trace-ids`到行中，你将看到 curl 如何为所有跟踪添加两个数字：连接号和传输号。它们是两个独立的标识符，因为连接可以被重用，多个传输可以使用相同的连接。

## 更多数据

如果跟踪数据量不足。例如，当你怀疑并想要在更基础的底层协议级别调试问题时，curl 为你提供了`--trace-config`选项。

使用这个选项，你告诉 curl 也记录它默认不包含的组件的日志，例如 TLS、HTTP/2 或 HTTP/3 协议的详细信息。它还提供了方便的选项来添加连接和传输标识符以及时间戳。

`--trace-config`选项接受一个参数，其中你可以指定一个以逗号分隔的列表，指定你想要跟踪的区域。例如，包括标识符并显示 HTTP/2 的详细信息：

```sh
curl --trace-config ids,http/2 https://example.com
```

选项的确切集合各不相同，但这里有一些可以尝试的选项：

| area | description |
| --- | --- |
| `ids` | 与`--trace-ids`提供的相同标识符 |
| `time` | 与`--trace-time`提供的相同时间输出 |
| `all` | 显示所有可能的内容 |
| `tls` | TLS 协议交换详情 |
| `http/2` | HTTP/2 帧信息 |
| `http/3` | HTTP/3 帧信息 |
| `*` | 未来版本中可能添加的额外信息 |

使用 `all` 进行快速运行通常是一个很好的方法来查看显示的具体区域，因为这样你可以进行后续的运行，并设置更具体的区域。
