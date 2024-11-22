# 检查哪个进程在你的 MAC 或 Linux 本地计算机上使用端口 8080

> 原文：[`techbyexample.com/process-using-8080/`](https://techbyexample.com/process-using-8080/)

**lsof**命令可以用来检查哪个进程正在占用端口 8080。**lsof**命令代表**List Of Open File**（打开文件列表）。

它提供了一个**-i**选项，可以用来检查系统中可能已被某些网络连接打开的文件。它列出了所有的 UDP 和 TCP 连接。

输入以下命令

```go
lsof -i tcp:8080
```

它会告诉你哪个进程正在占用端口 8080。以下是我系统上的输出。

```go
COMMAND   PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
main    35631 <user>    3u  IPv6 0x5a460c6a5bab1b15      0t0  TCP *:http-alt (LISTEN)
```

输出中还包含了进程的 PID，我可以用这个 PID 来终止该进程。以下是终止进程的命令：

```go
kill -9 <pid_of_process>
```

+   [8080](https://techbyexample.com/tag/8080/)*   [linux](https://techbyexample.com/tag/linux/)*   [mac](https://techbyexample.com/tag/mac/)*   [port](https://techbyexample.com/tag/port/)
