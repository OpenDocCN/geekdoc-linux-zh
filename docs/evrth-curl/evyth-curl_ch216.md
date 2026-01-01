# 进度信息

进度回调是在整个传输过程中，对于每次传输都会定期和重复调用的。旧的回调是通过`CURLOPT_PROGRESSFUNCTION`设置的，但现代且首选的回调是通过`CURLOPT_XFERINFOFUNCTION`设置的：

```sh
curl_easy_setopt(handle, CURLOPT_XFERINFOFUNCTION, xfer_callback);
```

`xfer_callback`函数必须匹配以下原型：

```sh
int xfer_callback(void *clientp, curl_off_t dltotal, curl_off_t dlnow,
                  curl_off_t ultotal, curl_off_t ulnow);
```

如果设置了此选项且`CURLOPT_NOPROGRESS`设置为 0（零），则 libcurl 会以频繁的间隔调用此回调函数。在数据传输过程中，它会被频繁调用，而在没有数据传输的缓慢时期，它可以慢到大约每秒调用一次。

`clientp`指针指向通过`CURLOPT_XFERINFODATA`设置的私有数据：

```sh
curl_easy_setopt(handle, CURLOPT_XFERINFODATA, custom_pointer);
```

回调会被告知 libcurl 即将传输和已传输的数据量，以字节数表示：

+   `dltotal`是 libcurl 在此传输中预期下载的总字节数。

+   `dlnow`是到目前为止已下载的字节数。

+   `ultotal`是 libcurl 在此传输中预期上传的总字节数。

+   `ulnow`是到目前为止已上传的字节数。

传递给回调的未知/未使用参数值被设置为零（例如，如果您只下载数据，上传大小保持为零）。很多时候，在知道数据大小之前，回调会被调用一次或多次，因此必须编写程序来处理这种情况。

从此回调返回非零值会导致 libcurl 终止传输并返回`CURLE_ABORTED_BY_CALLBACK`。

如果您使用多接口传输数据，除非您调用执行传输的适当 libcurl 函数，否则在空闲期间不会调用此函数。

（已弃用的回调`CURLOPT_PROGRESSFUNCTION`工作方式相同，但它使用的是`double`类型，而不是`curl_off_t`类型。）
