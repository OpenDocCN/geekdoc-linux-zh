# 引用者

当用户在网页上点击链接，浏览器将用户带到下一个 URL 时，它会向新的 URL 发送一个包含`Referer:`头部的请求，告知其来源。这就是引用者头部。`Referer:`拼写错误，但这就是它应有的样子。

使用 curl，你可以通过`-e`或`--referer`设置引用者头部，如下所示：

```sh
curl --referer http://comes-from.example.com https://www.example.com/
```
