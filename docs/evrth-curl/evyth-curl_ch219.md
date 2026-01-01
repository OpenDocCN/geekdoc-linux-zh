# sockopt

sockopt 回调通过 `CURLOPT_SOCKOPTFUNCTION` 设置：

```sh
curl_easy_setopt(handle, CURLOPT_SOCKOPTFUNCTION, sockopt_callback);
```

`sockopt_callback` 函数必须匹配此原型：

```sh
int sockopt_callback(void *clientp,
                     curl_socket_t curlfd,
                     curlsocktype purpose);
```

当 libcurl 创建了一个新的套接字但在连接调用之前，这个回调函数会被调用，以便应用程序更改特定的套接字选项。

**clientp** 指针指向使用 `CURLOPT_SOCKOPTDATA` 设置的私有数据集：

```sh
curl_easy_setopt(handle, CURLOPT_SOCKOPTDATA, custom_pointer);
```

此回调应返回：

+   CURL_SOCKOPT_OK 表示成功

+   CURL_SOCKOPT_ERROR 用于向 libcurl 信号不可恢复的错误

+   CURL_SOCKOPT_ALREADY_CONNECTED 表示成功，同时也表明套接字实际上已经连接到目标
