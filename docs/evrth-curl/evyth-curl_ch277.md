# 写入

应用程序可以通过两种不同的方式接收 WebSocket 数据，但发送数据的方式只有一种：`curl_ws_send()`函数。

## `curl_ws_send()`

```sh
CURLcode curl_ws_send(CURL *curl, const void *buffer, size_t buflen,
                      size_t *sent, curl_off_t fragsize,
                      unsigned int sendflags);
```

`curl` - 传输句柄

`buffer` - 指向要发送的帧数据的指针

`buflen` - `buffer`中数据的长度（以字节为单位）

`fragsize` - 整个片段的总大小，用于发送较大片段的一部分。

`sent` - 发送的字节数

`flags` - 描述数据的位掩码。请参见下面的位描述。

## 完整片段与部分片段

要发送完整的 WebSocket 片段，将`fragsize`设置为零，并为所有其他参数提供数据。

要以更小的片段发送片段：使用`fragsize`设置为总片段大小发送第一部分。你**必须**知道并提供整个片段的大小，然后才能发送它。在随后的`curl_ws_send()`调用中，使用`fragsize`设置为零但在`flags`参数中设置`CURLWS_OFFSET`位发送片段的下一部分。重复此操作，直到发送构成整个片段的所有部分。

## 标志

### `CURLWS_TEXT`

缓冲区包含文本数据。请注意，这会对 WebSocket 产生影响，但 libcurl 本身并不对内容进行任何验证，也不会采取任何预防措施来确保你发送的是有效的 UTF-8 内容。

### `CURLWS_BINARY`

这是二进制数据。

### `CURLWS_CONT`

这不是消息的最终片段，这暗示了作为同一消息的一部分，还有另一个片段即将到来，其中此位未设置。

### `CURLWS_CLOSE`

关闭此传输。

### `CURLWS_PING`

这作为 ping。

### `CURLWS_PONG`

这作为回显。

### `CURLWS_OFFSET`

提供的数据仅是部分片段，还有更多数据将在随后的`curl_ws_send()`调用中到来。当仅以这种方式发送片段的一部分时，`fragsize`必须在第一次调用中提供总预期的帧大小，并且在后续调用中需要设置为零。

当`CURLWS_OFFSET`被设置时，不应设置其他标志位，因为这将是之前发送的延续，并且当时已设置了描述片段的位。
