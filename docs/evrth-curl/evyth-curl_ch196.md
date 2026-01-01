# CURLcode 返回代码

许多 libcurl 函数返回一个 CURLcode。这是一个特殊的 libcurl 类型定义变量，用于错误代码。如果没有问题，它返回`CURLE_OK`（其值为零），如果检测到问题，则返回非零数字。目前有近一百个`CURLcode`错误在使用中，您可以在`curl/curl.h`头文件中找到它们，并在 libcurl-errors 手册页中进行了文档化。

您可以使用`curl_easy_strerror()`函数将 CURLcode 转换为人类可读的字符串——但请注意，这些错误很少以适合在 UI 中公开或向最终用户展示的方式措辞：

```sh
const char *str = curl_easy_strerror( error );
printf("libcurl said %s\n", str);
```

在出现错误的情况下，获取稍微更好的错误文本的另一种方法是设置`CURLOPT_ERRORBUFFER`选项来指向程序中的一个缓冲区，然后 libcurl 在返回错误之前将相关错误消息存储在那里：

```sh
char error[CURL_ERROR_SIZE]; /* needs to be at least this big */
CURLcode ret = curl_easy_setopt(handle, CURLOPT_ERRORBUFFER, error);
```
