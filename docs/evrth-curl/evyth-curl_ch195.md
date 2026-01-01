# 多线程

libcurl 是线程安全的，但没有内部线程同步。你可能需要提供自己的锁定机制或更改选项以正确使用 libcurl 的多线程功能。具体需要什么取决于 libcurl 的构建方式。请参阅 [libcurl 线程安全](https://curl.se/libcurl/c/threadsafe.html) 页面，其中包含最新信息。
