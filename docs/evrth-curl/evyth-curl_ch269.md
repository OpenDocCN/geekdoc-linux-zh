# CURLOPT_CURLU

为了方便应用程序，它们可以将已经解析的 URL 传递给 libcurl 进行处理，作为`CURLOPT_URL`的替代方案。

使用`CURLOPT_CURLU`选项，您传递一个`CURLU`句柄而不是 URL 字符串。

示例：

```sh
CURLU *h = curl_url();
rc = curl_url_set(h, CURLUPART_URL, "https://example.com/", 0);

CURL *easy = curl_easy_init();
curl_easy_setopt(easy, CURLOPT_CURLU, h);
```
