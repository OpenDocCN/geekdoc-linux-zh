# 读取

应用程序可以通过以下两种方法之一接收和读取传入的 WebSocket 流量：

## 写入回调

当未设置 `CURLOPT_CONNECT_ONLY` 选项时，WebSocket 数据将传递给写入回调。

在默认帧模式（与原始模式相对），libcurl 在数据到达时将 WebSocket 数据片段的部分传递给回调。然后应用程序可以调用 `curl_ws_meta()` 来获取传递给回调的特定帧的信息。

libcurl 可以传递完整的片段或部分片段，这取决于何时通过网络传输的内容。每个 WebSocket 片段的大小可以达到 63 位。

## `curl_ws_recv`

如果设置了仅连接选项，则在将 WebSocket 设置到远程主机后，传输结束，从那时起应用程序需要调用 `curl_ws_recv()` 来读取 WebSocket 数据，并调用 `curl_ws_send()` 来发送它。

`curl_ws_recv` 函数具有以下原型：

```sh
CURLcode curl_ws_recv(CURL *curl, void *buffer, size_t buflen,
                      size_t *recv, struct curl_ws_frame **meta);
```

`curl` - 转移的句柄

`buffer` - 指向一个缓冲区的指针，用于接收 WebSocket 数据

`buflen` - 缓冲区的大小（以字节为单位）

`recv` - 返回时存储在 **buffer** 中的数据的大小（以字节为单位）

`meta` - 获取一个指向包含 接收帧信息 的结构的指针。
