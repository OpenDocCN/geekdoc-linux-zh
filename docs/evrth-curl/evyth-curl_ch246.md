# 响应

每个 HTTP 请求都包含一个 HTTP 响应。HTTP 响应是一组元数据和响应体，其中体有时可以是零字节，因此不存在。然而，HTTP 响应总是有响应头。

## 响应体

响应体传递给写入回调，响应头传递给头部回调。

几乎所有使用 libcurl 的应用程序都需要设置至少一个回调，指示 libcurl 如何处理接收到的头和数据。

## 响应元数据

libcurl 提供了`curl_easy_getinfo()`函数，允许应用程序查询 libcurl 关于之前执行传输的信息。

有时候一个应用程序只想知道数据的大小。响应的大小*如服务器头部所述*可以通过`curl_easy_getinfo()`提取，如下所示：

```sh
curl_off_t size;
curl_easy_getinfo(curl, CURLINFO_CONTENT_LENGTH_DOWNLOAD_T, &size);
```

如果你可以在传输完成后等待，这通常是一个更可靠的方法，因为并非所有 URL 都提前提供大小（例如，对于按需生成内容的服务器），你可以改为请求最近传输中下载的数据量。

```sh
curl_off_t size;
curl_easy_getinfo(curl, CURLINFO_SIZE_DOWNLOAD_T, &size);
```

## HTTP 响应代码

每个 HTTP 响应都以一行开始，该行包含 HTTP 响应代码。这是一个三位数，包含服务器对请求状态的想法。这些数字在 HTTP 标准规范中有详细说明，但它们被分为如下范围的类别：

| 代码 | 含义 |
| --- | --- |
| 1xx | 临时代码，后面跟一个新代码 |
| 2xx | 事情正常 |
| 3xx | 内容在其他地方 |
| 4xx | 由于客户端问题而失败 |
| 5xx | 由于服务器问题而失败 |

你可以在传输后像这样提取响应代码

```sh
long code;
curl_easy_getinfo(curl, CURLINFO_RESPONSE_CODE, &code);
```

## 关于 HTTP 响应代码“错误”

虽然响应代码数字可以包括（在 4xx 和 5xx 范围内）服务器用来表示请求处理过程中出现错误的数字，但重要的是要认识到这并不意味着 libcurl 会返回错误。

当 libcurl 被要求执行 HTTP 传输时，如果该 HTTP 传输失败，它会返回一个错误。然而，收到 HTTP 404 或类似错误对 libcurl 来说并不是问题。这不是 HTTP 传输错误。用户可能正在编写一个客户端来测试服务器的 HTTP 响应。

如果你坚持让 curl 将 HTTP 响应代码从 400 及以上视为错误，libcurl 提供了`CURLOPT_FAILONERROR`选项，如果设置，将指示 curl 在这种情况下返回`CURLE_HTTP_RETURNED_ERROR`。然后它尽快返回错误，并且不传递响应体。
