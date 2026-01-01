# TELNET

Telnet 是一种古老的应用协议，用于双向 **明文** 通信。它被设计用于交互式文本通信，*Telnet 没有加密或安全版本*。

TELNET 并不是 curl 的完美匹配。该协议并没有设计来处理纯上传或下载，因此 curl 的常规范例不得不进行一些调整，以便 curl 能够适当地处理它。

curl 将接收到的数据发送到 stdout，并从 stdin 读取输入以发送。当连接断开或用户按下控制-c 时，传输完成。

## 历史上的 TELNET

从前，系统提供 telnet 访问用于登录。然后您可以连接到服务器并登录，就像您今天使用 SSH 一样。由于该协议的不安全性，这种做法幸运地现在已经大部分被移入博物馆的陈列柜中。

Telnet 的默认端口号是 23。

## 使用 TELNET 调试

事实是 TELNET 实际上只是一个简单的明文 TCP 连接到目标主机和端口，这使得它在某些时候对调试其他协议和服务有一定的用处。

例如，连接到您的本地 HTTP 服务器在端口 80，并通过手动输入 `GET /` 并按两次回车来向它发送一个（损坏的）请求：

```sh
curl telnet://localhost:80
```

您的 Web 服务器可能返回类似以下内容：

```sh
HTTP/1.1 400 Bad Request
Date: Tue, 07 Dec 2021 07:41:16 GMT
Server: softeare/7.8.9
Content-Length: 31
Connection: close
Content-Type: text/html

[message]
```

## 选项

当 curl 设置到服务器的 TELNET 连接时，您可以要求它传递选项。您可以通过 `--telnet-option`（或 `-t`）来完成此操作，并且有三个选项可供使用：

+   `TTYPE=<term>` 将会话的“终端类型”设置为 `<term>`。

+   `XDISPLOC=<X display>` 设置 X 显示位置

+   `NEW_ENV=<var,val>` 将远程会话中的环境变量 `var` 设置为值 `val`

登录到您本地机器的 telnet 服务器，并告诉它您使用的是 `vt100` 终端：

```sh
curl --telnet-option TTYPE=vt100 telnet://localhost
```

当提示时，您需要手动输入您的用户名和密码。
