# 标头数据

标头回调是通过 `CURLOPT_HEADERFUNCTION` 设置的：

```sh
curl_easy_setopt(handle, CURLOPT_HEADERFUNCTION, header_callback);
```

`header_callback` 函数必须匹配以下原型：

```sh
size_t header_callback(char *ptr, size_t size, size_t nmemb,
                       void *userdata);
```

这个回调函数会在 libcurl 接收到标头后立即被调用。*ptr* 指向传递的数据，该数据的大小是 *size* 乘以 *nmemb*。libcurl 缓存标头，并逐个将“完整”的标头传递给这个回调。

传递给这个函数的数据不会被空终止。例如，你不能使用 printf 的 `%s` 操作符来显示内容，也不能使用 strcpy 来复制它。

这个回调应该返回实际处理的字节数。如果这个数字与传递给回调函数的数字不同，它将向库发出错误条件信号。这会导致传输中断，并且使用的 libcurl 函数返回 `CURLE_WRITE_ERROR`。

在 *userdata* 参数中传递给回调的用户指针是通过 `CURLOPT_HEADERDATA` 设置的：

```sh
curl_easy_setopt(handle, CURLOPT_HEADERDATA, custom_pointer);
```
