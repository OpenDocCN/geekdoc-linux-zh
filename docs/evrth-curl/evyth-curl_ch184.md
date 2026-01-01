# FTP 目录列表

您可以使用 curl 列出远程 FTP 目录，只需确保 URL 以反斜杠结尾。如果 URL 以斜杠结尾，curl 假定您想要列出的是一个目录。如果不是实际的目录，您可能会收到错误。

```sh
curl ftp://ftp.example.com/directory/
```

在 FTP 中，没有用于返回此类命令的标准目录输出语法，该命令使用标准的 FTP 命令`LIST`。列表通常是可读的，并且完全理解，但不同的服务器可能会使用略微不同的布局来返回列表。

要仅获取目录中所有名称的列表，从而避免常规目录列表的特殊格式，可以告诉 curl 使用`--list-only`（或简称`-l`）。然后 curl 会发出`NLST` FTP 命令：

```sh
curl --list-only ftp://ftp.example.com/directory/
```

NLST 有其独特的特点，因为一些 FTP 服务器在响应 NLST 时只列出实际的*文件*；它们不包括目录和符号链接。
