# 使用 FTP 上传

要上传到 FTP 服务器，您需要在 URL 中指定完整的目标文件路径和名称，并且使用`-T, --upload-file`指定要上传的本地文件名。可选地，您可以在目标 URL 的末尾加上一个斜杠，然后 curl 会将本地路径中的文件组件附加为远程文件名。

例如：

```sh
curl -T localfile ftp://ftp.example.com/dir/path/remote-file
```

或者使用本地文件名作为远程名称：

```sh
curl -T localfile ftp://ftp.example.com/dir/path/
```

curl 也支持在`-T`参数中使用通配符，这样您可以轻松上传一系列文件：

```sh
curl -T 'image[1-99].jpg' ftp://ftp.example.com/upload/
```

或者一系列文件：

```sh
curl -T '{file1,file2}' ftp://ftp.example.com/upload/
```

或者

```sh
curl -T '{Huey,Dewey,Louie}.jpg' ftp://ftp.example.com/nephews/
```
