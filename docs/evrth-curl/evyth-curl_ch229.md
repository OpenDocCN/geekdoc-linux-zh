# HSTS

对于 HSTS（HTTP 严格传输安全），libcurl 提供了两个回调函数，允许分配实现规则的存储。然后，这些回调函数被设置为从持久存储中读取和/或写入 HSTS 策略。

使用 `CURLOPT_HSTSREADFUNCTION`，应用程序提供了一个函数，通过该函数将 HSTS 数据读入 libcurl。`CURLOPT_HSTSWRITEFUNCTION` 是 libcurl 调用来写入数据的对应函数。
