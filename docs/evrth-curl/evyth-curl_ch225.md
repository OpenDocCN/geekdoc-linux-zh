# RTSP 交织数据

带有 `CURLOPT_INTERLEAVEFUNCTION` 选项的回调。

当进行 RTSP 传输时，libcurl 一旦收到交织的 RTP 数据就会调用此回调。它对每个 `$` 块进行调用，因此恰好包含一个上层协议单元（例如，一个 RTP 数据包）。libcurl 为每个调用写入交织的头部以及包含的数据。第一个字节始终是 ASCII 美元符号。美元符号后面跟着一个字节通道标识符，然后是一个 2 字节整数长度，以网络字节序表示。有关 RTP 交织行为的信息，请参阅 RFC2326 第 10.12 节。如果未设置或设置为 NULL，curl 将使用默认的写入函数。

`CURLOPT_INTERLEAVEDATA` 指针在回调函数的 userdata 参数中传递。
