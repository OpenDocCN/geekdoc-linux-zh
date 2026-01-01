# 读取数据

读取回调是通过 `CURLOPT_READFUNCTION` 设置的：

```sh
curl_easy_setopt(handle, CURLOPT_READFUNCTION, read_callback);
```

`read_callback` 函数必须匹配以下原型：

```sh
size_t read_callback(char *buffer, size_t size, size_t nitems,
                     void *stream);
```

当 libcurl 想要向服务器发送数据时，会调用此回调函数。这是一个您已设置用于上传数据或以其他方式将其发送到服务器的传输。此回调会在所有数据已发送或传输失败之前反复调用。

**stream** 指针指向通过 `CURLOPT_READDATA` 设置的私有数据集：

```sh
curl_easy_setopt(handle, CURLOPT_READDATA, custom_pointer);
```

如果没有设置此回调，libcurl 默认使用 'fread'。

指针 **buffer** 所指向的数据区域应由您的函数填充最多 **size** 乘以 **nitems** 个字节的字节。然后回调应返回它存储在该内存区域中的字节数，或者如果已达到数据末尾，则返回 0。回调还可以返回一些“魔法”返回代码，以使 libcurl 立即返回失败或暂停特定的传输。有关详细信息，请参阅 [CURLOPT_READFUNCTION 手册页](https://curl.se/libcurl/c/CURLOPT_READFUNCTION.html)。
