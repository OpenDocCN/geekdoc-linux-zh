# 非 HTTP 重定向

浏览器支持多种重定向方式，这些方式有时会让 curl 用户的生活变得复杂，因为这些方法不被 curl 支持或识别。

## HTML 重定向

如果上述内容还不够，网络世界还提供了一种通过纯 HTML 重定向浏览器的方法。请参见下面的示例 `<meta>` 标签。这对于 curl 来说有些复杂，因为 curl 从不解析 HTML，因此对这些类型的重定向没有了解。

```sh
<meta http-equiv="refresh" content="0; url=http://example.com/">
```

## JavaScript 重定向

现代网络充满了 JavaScript，正如你所知，JavaScript 是一种语言，也是一个完整的运行时环境，允许代码在访问网站时在浏览器中执行。

JavaScript 还提供了让浏览器跳转到另一个网站的手段——如果你愿意，可以称之为重定向。
