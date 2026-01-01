# 元数据

`curl_ws_recv()`和`curl_ws_meta()`都返回一个指向`curl_ws_frame`结构的指针，该结构提供了关于传入 WebSocket 数据的信息。在这种情况下，WebSocket“帧”是 WebSocket 片段的一部分。它*可以*是一个完整的片段，但也可能只是其中的一部分。`curl_ws_frame`包含有关帧的信息，以告知您详细信息。

```sh
struct curl_ws_frame {
  int age;              /* zero */
  int flags;            /* See the CURLWS_* defines */
  curl_off_t offset;    /* the offset of this data into the frame */
  curl_off_t bytesleft; /* number of pending bytes left of the payload */
};
```

## `age`

这只是一个标识此结构年龄的数字。现在它始终为 0，但将来可能会增加，那时结构可能会增长。

## `flags`

`flags`字段是一个位掩码，描述了数据的详细信息。

### `CURLWS_TEXT`

缓冲区包含文本数据。请注意，这会对 WebSocket 产生影响，但 libcurl 本身并不验证内容或采取任何预防措施来确保您接收到的确实是有效的 UTF-8 内容。

### `CURLWS_BINARY`

这是二进制数据。

### `CURLWS_FINAL`

这是消息的最后一个片段，如果未设置，则表示还有另一个片段作为同一消息的一部分即将到来。

### `CURLWS_CLOSE`

此传输现在已关闭。

### `CURLWS_PING`

这是一个接收到的 ping 消息，它期望得到一个 pong 响应。

## `offset`

当传递的数据只是更大片段的一部分时，这标识了此片段属于更大片段的字节数偏移量。

## `bytesleft`

在此帧之后，剩余未完成此片段的未处理负载字节数。

WebSocket 片段的最大大小为 63 位。
