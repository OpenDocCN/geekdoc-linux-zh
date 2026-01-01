# 网络数据转换

直到 libcurl 版本 7.82.0，这些回调被提供以使非 ASCII 平台上的事物能够工作。**自那时起，对这些回调的支持已被移除**。

以下文档暂时保留在此处，描述了它们过去是如何工作的。它将在未来的某个日期从本书中移除。

## 转换到和从网络回调

对于非 ASCII 平台，提供了 `CURLOPT_CONV_FROM_NETWORK_FUNCTION`。此函数应将网络编码**转换为**主机编码。

`CURLOPT_CONV_TO_NETWORK_FUNCTION` 应将主机编码**转换为**网络编码。它在通过网络发送命令或 ASCII 数据时使用。

## 从 UTF-8 回调转换

`CURLOPT_CONV_FROM_UTF8_FUNCTION` 应将 UTF-8 编码**转换为**主机编码。它仅适用于 SSL 处理。
