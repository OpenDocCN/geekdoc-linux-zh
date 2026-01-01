# 设置数值选项

由于 `curl_easy_setopt()` 是一个 vararg 函数，其中第 3 个参数可以根据情况使用不同类型，因此不能进行常规的 C 语言类型转换。您**必须**确保如果文档中这样说明，您确实传递了一个 `long` 而不是一个 `int`。在它们大小相同的架构上，您可能不会遇到任何问题，但并非所有的工作方式都如此。同样，对于接受 `curl_off_t` 类型的选项，您使用该类型传递参数是**至关重要的**，而不是其他任何类型。

强制使用 `long`：

```sh
curl_easy_setopt(handle, CURLOPT_TIMEOUT, 5L); /* 5 seconds timeout */
```

强制使用 `curl_off_t`：

```sh
curl_off_t no_larger_than = 0x50000;
curl_easy_setopt(handle, CURLOPT_MAXFILESIZE_LARGE, no_larger_than);
```
