# 转换为 GET

在这个上下文中可以提及的一个小便利功能是 `-G` 或 `--get` 选项，它接受您使用不同的 `-d` 变体指定的所有数据，并将这些数据附加到输入的 URL 上，例如 `http://example.com`，用问号分隔，然后让 curl 发送一个 GET 请求。

此选项使得在 POST 和 GET 表单之间切换变得容易，例如。

一个示例，将编码后的数据作为查询添加到 URL 中：

```sh
curl -G --data-urlencode "name=daniel stenberg" https://example.com/
```
