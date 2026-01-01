# 详细操作

好的，我们刚刚展示了如何将错误作为可读文本获取，这对于确定特定传输中出了什么问题非常有帮助，通常解释了为什么可以这样做或目前的问题是什么。

当编写 libcurl 应用程序时，每个人都需要了解并广泛使用的一个救星，至少在开发 libcurl 应用程序或调试 libcurl 本身时，是启用详细模式`CURLOPT_VERBOSE`：

```sh
CURLcode ret = curl_easy_setopt(handle, CURLOPT_VERBOSE, 1L);
```

当 libcurl 被要求详细输出时，它在传输进行时将传输相关的细节和信息输出到 stderr。这对于找出为什么失败以及了解当您要求它做不同的事情时 libcurl 到底做了什么非常有用。您可以通过更改 stderr 使用`CURLOPT_STDERR`将输出重定向到其他地方，或者您可以通过调试回调（在后面的部分中进一步解释）以更花哨的方式获取更多信息。

## 跟踪一切

详细模式当然很好，但有时您需要更多。libcurl 还提供了一个跟踪回调，除了显示详细模式所做的所有内容外，它还传递发送和接收的所有数据，以便您的应用程序获得完整的跟踪。

传递给跟踪回调的发送和接收数据以未加密的形式提供给回调，这在处理基于 TLS 或 SSH 的协议时很有用，当从网络上捕获数据以进行调试不切实际时。

当您设置`CURLOPT_DEBUGFUNCTION`选项时，您仍然需要启用`CURLOPT_VERBOSE`，但将跟踪回调设置为 libcurl 使用该回调而不是其内部处理。

跟踪回调应与以下原型匹配：

```sh
int my_trace(CURL *handle, curl_infotype type, char *data, size_t size,
             void *user);
```

**handle**是相关的简单句柄，**type**描述传递给回调的特定数据（数据输入/输出、头部输入/输出、TLS 数据输入/输出和文本），**data**是一个指向数据的指针，该数据是**size**字节数。**user**是您使用`CURLOPT_DEBUGDATA`设置的定制指针。

**data**指向的数据不是 null 终止的，但正好与**size**参数告诉的大小相同。

回调必须返回 0，否则 libcurl 将其视为错误并终止传输。

在 curl 网站上，我们托管了一个名为[debug.c](https://curl.se/libcurl/c/debug.html)的示例，其中包含一个简单的跟踪函数以供参考。

在[CURLOPT_DEBUGFUNCTION 手册页](https://curl.se/libcurl/c/CURLOPT_DEBUGFUNCTION.html)中还有额外的详细信息。

## 传输和连接标识符

尽管您的应用程序可能让 libcurl 使用大量单独的连接和不同的传输，但跟踪信息流传递给调试回调时是一个连续的流，有时您想查看各种信息属于哪些特定的传输或连接。为了更好地理解跟踪输出。

您可以在回调内部获取传输和连接标识符：

```sh
curl_off_t conn_id;
curl_off_t xfer_id;
res = curl_easy_getinfo(curl, CURLINFO_CONN_ID, &conn_id);
res = curl_easy_getinfo(curl, CURLINFO_XFER_ID, &xfer_id);
```

它们是两个独立的标识符，因为连接可以被重用，多个传输可以使用相同的连接。使用这些标识符（实际上是数字），你可以看到哪些日志与哪些传输和连接相关联。

## 跟踪更多

如果传递给调试回调函数的默认跟踪数据量不足。例如，当你怀疑并想要在更基础的底层协议级别调试问题时，libcurl 为你提供了 `curl_global_trace()` 函数。

使用此函数，你告诉 libcurl 也包括关于它默认不包含的组件的详细日志。例如，关于 TLS、HTTP/2 或 HTTP/3 协议的详细信息。

`curl_global_trace()` 函数接受一个参数，其中你指定一个字符串，包含你想要它跟踪的区域以逗号分隔的列表。例如，包括 TLS 和 HTTP/2 的详细信息：

```sh
/* log details of HTTP/2 and SSL handling */
curl_global_trace("http/2,ssl");
```

具体的选项集可能会有所不同，但这里有一些可以尝试的选项：

| area | 描述 |
| --- | --- |
| `all` | 显示所有可能的内容 |
| `tls` | TLS 协议交换细节 |
| `http/2` | HTTP/2 帧信息 |
| `http/3` | HTTP/3 帧信息 |
| `*` | 未来版本中的附加选项 |

使用 `all` 快速运行通常是一个很好的方法来查看哪些特定区域被显示，因为这样你可以进行后续的运行，并设置更具体的区域。
