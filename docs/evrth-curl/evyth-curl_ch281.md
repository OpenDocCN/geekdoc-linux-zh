# 遍历头信息

```sh
struct curl_header *curl_easy_nextheader(CURL *easy,
                                         unsigned int origin,
                                         int request,
                                         struct curl_header *previous);
```

此函数允许应用程序遍历给定**来源**中所有可用的头信息。

**请求**参数告诉 libcurl 从哪个请求中获取头信息。

如果**previous**设置为 NULL，此函数将返回第一个头信息的指针。然后应用程序可以使用该指针作为下一个调用的参数，遍历同一**来源**和**请求**上下文中的所有可用头信息。

当此函数返回 NULL 时，上下文中没有更多的头信息。

请参阅头结构，了解该函数返回的`curl_header`结构体的详细信息。
