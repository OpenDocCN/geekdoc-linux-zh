# 下载

GET 方法是 libcurl 在请求 HTTP URL 且未指定特定其他方法时的默认方法。它请求服务器提供特定资源——标准的 HTTP 下载请求：

```sh
easy = curl_easy_init();
curl_easy_setopt(easy, CURLOPT_URL, "http://example.com/");
curl_easy_perform(easy);
```

由于在 easy 处理程序中设置的选项是粘性的，并且会保持直到被更改，因此可能会有这样的情况：你请求了另一种请求方法而不是 GET，然后又想切换回 GET 以进行后续请求。为此，存在 `CURLOPT_HTTPGET` 选项：

```sh
curl_easy_setopt(easy, CURLOPT_HTTPGET, 1L);
```

## 下载头部信息

HTTP 传输还包括一组响应头部。响应头部是与实际有效载荷（称为响应正文）关联的元数据。所有下载都会获得一组头部信息，但当你使用 libcurl 时，你可以选择是否希望下载（查看）它们。

你可以通过使用 `CURLOPT_HEADER` 来请求 libcurl 将头部信息传递到与常规正文相同的流中：

```sh
easy = curl_easy_init();
curl_easy_setopt(easy, CURLOPT_HEADER, 1L);
curl_easy_setopt(easy, CURLOPT_URL, "http://example.com/");
curl_easy_perform(easy);
```

或者，你也可以选择将头部信息存储在单独的下载文件中，这依赖于 write 和 header callbacks 的默认行为：

```sh
easy = curl_easy_init();
FILE *file = fopen("headers", "wb");
curl_easy_setopt(easy, CURLOPT_HEADERDATA, file);
curl_easy_setopt(easy, CURLOPT_URL, "http://example.com/");
curl_easy_perform(easy);
fclose(file);
```

如果你只想随意浏览头部信息，那么在开发时仅设置详细模式可能就足够让你满意了，因为这样会在标准错误输出中显示发送的出站和入站头部信息：

```sh
curl_easy_setopt(easy, CURLOPT_VERBOSE, 1L);
```
