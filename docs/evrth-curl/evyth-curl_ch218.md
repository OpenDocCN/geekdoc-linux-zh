# 调试

通过`CURLOPT_DEBUGFUNCTION`设置调试回调：

```sh
curl_easy_setopt(handle, CURLOPT_DEBUGFUNCTION, debug_callback);
```

`debug_callback`函数必须匹配此原型：

```sh
int debug_callback(CURL *handle,
                   curl_infotype type,
                   char *data,
                   size_t size,
                   void *userdata);
```

此回调函数替换了库中的默认详细输出函数，并在所有调试和跟踪消息中被调用，以帮助应用程序理解正在发生的事情。*类型*参数解释了提供的数据类型：标题、数据或 SSL 数据以及其流动方向。

此回调的常见用途是获取 libcurl 发送和接收的所有数据的完整跟踪。发送到此回调的数据始终是未加密版本，即使使用 HTTPS 或其他加密协议也是如此。

此回调必须返回零或使用错误代码停止传输。

在*userdata*参数中传递给回调的用户指针通过`CURLOPT_DEBUGDATA`设置：

```sh
curl_easy_setopt(handle, CURLOPT_DEBUGDATA, custom_pointer);
```
