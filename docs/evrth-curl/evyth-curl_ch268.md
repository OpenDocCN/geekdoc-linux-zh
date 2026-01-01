# 添加到查询

应用程序可以使用 `CURLU_APPENDQUERY` 标志将字符串添加到现有查询部分的右侧。

考虑一个持有 URL `https://example.com/?shoes=2` 的句柄。应用程序可以像这样将字符串 `hat=1` 添加到查询部分：

```sh
rc = curl_url_set(urlp, CURLUPART_QUERY, "hat=1", CURLU_APPENDQUERY);
```

它甚至注意到了缺少 `&` 分隔符，因此也注入了一个，句柄的完整 URL 因此变为 `https://example.com/?shoes=2&hat=1`。

添加的字符串当然也可以在添加时进行 URL 编码，如果需要，编码会跳过 `=` 字符。例如，将 `candy=M&M` 添加到我们已有的内容中，并将其 URL 编码以处理数据中的 `&` 符号：

```sh
rc = curl_url_set(urlp, CURLUPART_QUERY, "candy=M&M",
                  CURLU_APPENDQUERY | CURLU_URLENCODE);
```

现在 URL 看起来像这样：`https://example.com/?shoes=2&hat=1&candy=M%26M`。
