# PAC

一些网络环境提供了在不同情况下应使用的多个不同代理，浏览器支持一种可定制的处理方式。这被称为“[代理自动配置](https://en.wikipedia.org/wiki/Proxy_auto-config)”，或 PAC。

PAC 文件包含一个 JavaScript 函数，该函数决定给定的网络连接（URL）应使用哪个代理，甚至决定是否完全不使用代理。浏览器通常从本地网络上的 URL 读取 PAC 文件。

由于 curl 没有 JavaScript 功能，curl 不支持 PAC 文件。如果你的浏览器和网络使用 PAC 文件，通常最简单的途径是手动读取 PAC 文件并找出你需要指定以成功运行 curl 的代理。
