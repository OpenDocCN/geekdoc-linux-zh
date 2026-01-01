# 选项

应用程序有一个专门的 setopt 选项来控制 WebSocket 通信：`CURLOPT_WS_OPTIONS`。

此选项设置一个标志位的掩码给 libcurl，但到目前为止，只有一个位被使用。

## 原始模式

通过在掩码中设置 `CURLWS_RAW_MODE` 位，libcurl 将所有 WebSocket 流量以原始形式传递给写入回调，而不是自己解析 WebSocket 流量。这种原始模式适用于可能已经实现了 WebSocket 处理的应用程序，它们只想迁移到使用 libcurl 进行传输并保持自己的 WebSocket 逻辑。

在原始模式下，libcurl 也不会自动处理任何 PING 流量。
