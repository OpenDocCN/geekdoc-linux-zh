# 解析器启动

这个回调函数，通过`CURLOPT_RESOLVER_START_FUNCTION`设置，每次在 libcurl 开始一个新的解析请求之前都会被调用，并指定解析是为哪个`CURL *`句柄进行的。
