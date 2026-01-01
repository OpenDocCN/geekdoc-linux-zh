# 获取一个 URL

`CURLU *` 处理符代表一个单独的 URL，你可以使用 `curl_url_get` 函数轻松提取完整的 URL 或其各个部分：

```sh
char *url;
rc = curl_url_get(h, CURLUPART_URL, &url, CURLU_NO_DEFAULT_PORT);
curl_free(url);
```

如果处理符没有足够的信息来返回所请求的部分，它将返回错误。

在使用完毕后，必须使用 `curl_free()` 函数释放返回的字符串。
