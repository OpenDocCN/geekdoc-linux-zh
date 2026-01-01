# 标志

当使用 `curl_url_get()` 检索 URL 部分时，API 提供了一些不同的切换选项，以更好地指定应如何返回该内容。它们设置在 `flags` 位掩码参数中，这是函数的第四个参数。你可以设置零、一个或多个位。

## `CURLU_DEFAULT_PORT`

如果 URL 句柄没有存储端口号，此选项使 `curl_url_get()` 返回用于该方案的默认端口号。

## `CURLU_DEFAULT_SCHEME`

如果句柄没有存储方案，此选项使 `curl_url_get()` 返回默认方案而不是错误。

## `CURLU_NO_DEFAULT_PORT`

指示 `curl_url_get()` 在生成的 URL 中不使用端口号，如果该端口号与用于方案的默认端口号匹配。例如，如果端口号设置为 443，方案为 `https`，则提取的 URL 不包括端口号。

## `CURLU_URLENCODE`

此标志使 `curl_url_get()` 在检索完整 URL 时对主机名部分进行 URL 编码。如果没有设置（默认），libcurl 以“raw”形式返回 URL，以支持 IDN 名称以原始形式出现。IDN 主机名通常使用非 ASCII 字节，否则将进行百分编码。

注意，即使不请求 URL 编码，`%`（字节 37）在主机名中也是 URL 编码的，以确保主机名保持有效。

## `CURLU_URLDECODE`

告诉 `curl_url_get()` 在返回之前对内容进行 URL 解码。它确实尝试解码方案、端口号或完整 URL。当设置此位时，查询组件还会获得加号到空格的转换作为额外的好处。请注意，此 URL 解码是无字符集意识的，你将获得一个以零终止的字符串，其中包含可能旨在特定编码的数据。如果解码字符串中存在任何小于 32 的字节值，则获取操作将返回错误。

## `CURLU_PUNYCODE`

如果设置且未设置 `CURLU_URLENCODE`，并且请求检索 `CURLUPART_HOST` 或 `CURLUPART_URL` 部分，当主机名包含任何非 ASCII 八进制数（并且是 IDN 名称）时，libcurl 返回其 punycode 版本。如果 libcurl 没有构建 IDN 功能，使用此位使 `curl_url_get()` 在主机名包含 ASCII 范围之外的任何内容时返回 `CURLUE_LACKS_IDN`。
