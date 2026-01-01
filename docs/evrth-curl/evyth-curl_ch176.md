# User-agent

User-Agent 是一个头部，每个客户端都可以在请求中设置，以通知服务器它使用的用户代理。有时服务器会查看这个头部并根据其内容决定如何行动。

默认的头部值是‘curl/版本’，例如对于 curl 版本 7.54.1，显示为`User-Agent: curl/7.54.1`。

您可以设置任何喜欢的值，使用选项`-A`或`--user-agent`加上要使用的字符串，或者，因为它只是一个头部，可以使用`-H "User-Agent: foobar/2000"`。

作为比较，Linux 机器上 Firefox 的一个测试版本曾经发送过这个 User-Agent 头部：

`User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:58.0) Gecko/20100101 Firefox/58.0`
