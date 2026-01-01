# 支持

WebSocket 是 libcurl 7.86.0 及更高版本中存在的一个 **实验性** 功能。由于它是实验性的，您需要在构建时显式启用它，才能使其存在并可用。

要确定您的 libcurl 安装是否支持 WebSocket，您可以调用 `curl_version_info()` 函数（见 ch192.xhtml#libcurl__api__md），并检查返回的结构体中的 `->protocols` 字段。它应该包含 `ws` 以表明其存在，可能还会包含 `wss`。
