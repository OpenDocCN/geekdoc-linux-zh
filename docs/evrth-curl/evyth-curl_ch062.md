# FTP 类型

这不是一个广泛使用的功能。

用于标识 FTP 服务器上文件的 URL 具有一个特殊功能，允许您同时告知客户端（在这种情况下为 curl）资源的文件类型。这是因为 FTP 有一些特别之处，可以改变传输模式，从而以不同于其他模式的方式处理文件。

您可以通过在 URL 后附加`;type=A`来告诉 curl FTP 资源是 ASCII 类型。使用 ASCII 从`example.com`的根目录获取`foo`文件可以这样实现：

```sh
curl "ftp://example.com/foo;type=A"
```

尽管 curl 默认为 FTP 使用二进制传输，但 URL 格式允许您通过 type=I 指定二进制类型：

```sh
curl "ftp://example.com/foo;type=I"
```

最后，如果您传递的类型是 D，您可以告诉 curl 所标识的资源是一个目录。

```sh
curl "ftp://example.com/foo;type=D"
```

…这样就可以作为一个替代格式，而不是像上面提到的那样以路径结束符结束。
