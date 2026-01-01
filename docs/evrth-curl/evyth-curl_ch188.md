# 目录遍历

当执行 FTP 命令遍历远程文件系统时，curl 可以采取几种不同的方式来达到目标文件，即用户想要传输的文件。

## multicwd

curl 可以为文件树层次结构中的每个单独目录执行一个更改目录（CWD）命令。如果完整路径是`one/two/three/file.txt`，那么这种方法意味着在请求传输`file.txt`文件之前执行三个`CWD`命令。因此，如果路径有很多层，这种方法会创建相当多的命令。这种方法由早期规范（RFC 1738）规定，并且是 curl 默认的行为：

```sh
curl --ftp-method multicwd ftp://example.com/one/two/three/file.txt
```

这就等于这个 FTP 命令/响应序列（简化版）：

```sh
> CWD one
< 250 OK. Current directory is /one
> CWD two
< 250 OK. Current directory is /one/two
> CWD three
< 250 OK. Current directory is /one/two/three
> RETR file.txt
```

## nocwd

与为每个目录部分执行一个`CWD`命令相反，是根本不更改目录。这种方法一次性使用整个路径请求服务器，因此速度快。偶尔服务器会对此有问题，并且它并不完全符合标准：

```sh
curl --ftp-method nocwd ftp://example.com/one/two/three/file.txt
```

这就等于这个 FTP 命令/响应序列（简化版）：

```sh
> RETR one/two/three/file.txt
```

## singlecwd

这位于其他两种 FTP 方法之间。它向目标目录发送一个单独的`CWD`命令，然后请求指定的文件：

```sh
curl --ftp-method singlecwd ftp://example.com/one/two/three/file.txt
```

这就等于这个 FTP 命令/响应序列（简化版）：

```sh
> CWD one/two/three
< 250 OK. Current directory is /one/two/three
> RETR file.txt
```
