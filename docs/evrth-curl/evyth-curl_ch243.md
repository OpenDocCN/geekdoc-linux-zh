# 清理

在前面的章节中，我们讨论了如何设置 handles 以及如何驱动传输。所有的传输最终都会结束，无论是成功还是失败。

## 多 API

当你使用多 API 完成单个传输后，你使用`curl_multi_info_read()`来识别哪个 easy handle 已经完成，然后使用`curl_multi_remove_handle()`将其从 multi handle 中移除。

如果你从 multi handle 中移除了最后一个 easy handle，使得没有更多的传输在进行，你可以这样关闭 multi handle：

```sh
curl_multi_cleanup( multi_handle );
```

## easy handle

当 easy handle 完成其服务目的后，你可以关闭它。然而，如果你打算进行另一个传输，建议你重用 handle 而不是关闭它并创建一个新的。

如果你没有打算使用 easy handle 进行另一个传输，你只需请求 libcurl 进行清理：

```sh
curl_easy_cleanup( easy_handle );
```
