# 重定向到 URL

当句柄已经解析了一个 URL 时，设置第二个相对 URL 会使它“重定向”以适应它。

例如，首先设置原始 URL，然后设置我们要“重定向”到的 URL：

```sh
CURLU *h = curl_url();
rc = curl_url_set(h, CURLUPART_URL,
                  "https://example.com/foo/bar?name=moo", 0);

rc = curl_url_set(h, CURLUPART_URL, "../test?another", 0);
```
