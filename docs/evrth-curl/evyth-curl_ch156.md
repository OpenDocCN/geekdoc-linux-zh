# Content-Type

使用 curl 的`-d`选项进行 POST 操作时，它会包含一个默认的头部信息，看起来像`Content-Type: application/x-www-form-urlencoded`。这就是你典型的浏览器用于普通 POST 请求的方式。

许多接收 POST 数据的接收者并不关心或检查`Content-Type`头部信息。

如果这个头部信息对你来说不够好，你应该当然地替换它，并提供正确的一个。例如，如果你向服务器发送 JSON 数据，并希望更准确地告诉服务器内容类型：

```sh
curl -d '{json}' -H 'Content-Type: application/json' https://example.com
```
