# HTTP PUT

PUT 和 POST 之间的区别很微妙。它们在传输上几乎相同，只是方法字符串不同。POST 旨在将数据传递给远程资源，而 PUT 则应该是该资源的新版本。

在这方面，PUT 与在其他协议中找到的古老标准文件上传类似。您使用 PUT 上传资源的最新版本。URL 标识资源，您指出要放置的本地文件：

```sh
curl -T localfile http://example.com/new/resource/file
```

`-T` 表示一个 PUT 操作，并告诉 curl 发送哪个文件。但 POST 和 PUT 之间的相似性也允许您通过使用常规 curl POST 机制（使用`-d`）发送一个 PUT 请求，但要求使用 PUT 方法：

```sh
curl -d "data to PUT" -X PUT http://example.com/new/resource/file
```
