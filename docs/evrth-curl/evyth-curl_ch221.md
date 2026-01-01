# 寻找和 ioctl

此回调函数通过`CURLOPT_SEEKFUNCTION`设置。

回调函数由 libcurl 调用，用于在输入流中定位到特定位置，并可用于在恢复上传时快速前进文件（而不是使用常规的读取函数/回调读取所有已上传的字节）。它还用于在已向服务器发送数据并需要再次发送数据时回滚流。这可能在执行带有多阶段认证方法的 HTTP PUT 或 POST 时发生，或者当现有的 HTTP 连接太晚重用时服务器关闭连接。该函数应像 fseek(3)或 lseek(3)一样工作，并接收`SEEK_SET`、`SEEK_CUR`或`SEEK_END`作为原点的参数，尽管 libcurl 当前只传递`SEEK_SET`。

发送到回调函数的`userp`是您使用`CURLOPT_SEEKDATA`设置的指针。

回调函数在成功时必须返回`CURL_SEEKFUNC_OK`，在导致上传操作失败时返回`CURL_SEEKFUNC_FAIL`，或在寻求失败时返回`CURL_SEEKFUNC_CANTSEEK`以指示虽然寻求失败，但如果可能，libcurl 可以自由地绕过问题。后者有时可以通过从输入读取或类似的方式完成。

如果您直接将输入参数传递给 fseek(3)或 lseek(3)，请注意，偏移量的数据类型与许多系统上定义的`curl_off_t`不同。
