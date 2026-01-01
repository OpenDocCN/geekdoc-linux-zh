# 获取标题

```sh
CURLHcode curl_easy_header(CURL *easy,
                           const char *name,
                           size_t index,
                           unsigned int origin,
                           int request,
                           struct curl_header **hout);
```

此函数返回有关具有特定**名称**的字段的信息，并且您要求该函数在一个或多个**来源**中搜索它。

**index** 参数是当您想要请求标题的第 n 次出现时；当有多个可用时。将 **index** 设置为 0 返回第一个实例 - 在许多情况下，这可能是唯一的。

**request** 参数告诉 libcurl 您想要从哪个请求中获取标题。

应用程序需要在最后一个参数中传递一个指向 `struct curl_header *` 的指针，因为当没有错误返回时，会从那里返回一个指针。有关成功调用的 **out** 结果的详细信息，请参阅标题结构。

如果给定的名称与给定来源中接收到的任何标题都不匹配，则函数返回 `CURLHE_MISSING`，或者如果没有接收任何标题，则返回 `CURLHE_NOHEADERS`。
