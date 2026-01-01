# 设置 URL 部分

API 允许应用程序通过 `CURLU` 处理器设置 URL 的各个部分，无论是解析了完整的 URL 之后，还是不解析这样的 URL。

```sh
rc = curl_url_set(urlp, CURLUPART_HOST, "www.example.com", 0);
rc = curl_url_set(urlp, CURLUPART_SCHEME, "https", 0);
rc = curl_url_set(urlp, CURLUPART_USER, "john", 0);
rc = curl_url_set(urlp, CURLUPART_PASSWORD, "doe", 0);
rc = curl_url_set(urlp, CURLUPART_PORT, "443", 0);
rc = curl_url_set(urlp, CURLUPART_PATH, "/index.html", 0);
rc = curl_url_set(urlp, CURLUPART_QUERY, "name=john", 0);
rc = curl_url_set(urlp, CURLUPART_FRAGMENT, "anchor", 0);
rc = curl_url_set(urlp, CURLUPART_ZONEID, "25", 0);
```

API 总是期望在第三个参数中提供一个以空字符终止的 `char *` 字符串，或者 NULL 以清除字段。请注意，端口号也是以这种方式作为字符串提供的。

除非用户在第四个参数中使用 `CURLU_URLENCODE` 标志请求它，否则设置的各部分不会进行 URL 编码。

## 更新部分

通过设置单个部分，例如，你可以首先设置一个完整的 URL，然后更新该 URL 的单个组件，然后提取该 URL 的更新版本。

例如，假设我们有一个这样的 URL

```sh
const char *url="http://joe:7Hbz@example.com:8080/images?id=5445#footer";
```

我们希望将那个 URL 中的主机名更改为 `example.net`，可以这样做：

```sh
CURLU *h = curl_url();
rc = curl_url_set(h, CURLUPART_URL, url, 0);
```

然后更改主机名部分：

```sh
rc = curl_url_set(h, CURLUPART_HOST, "example.net", 0);
```

然后，现在这个 URL 如下所示：

```sh
http://joe:7Hbz@example.net:8080/images?id=5445#footer
```

如果你继续并将路径部分更改为 `/foo`，就像这样：

```sh
rc = curl_url_set(h, CURLUPART_PATH, "/foo", 0);
```

然后，URL 处理器现在持有这个 URL：

```sh
http://joe:7Hbz@example.net:8080/foo?id=5445#footer
```

等等……
