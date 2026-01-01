# 多次下载

由于 curl 可以告诉它在单个命令行中下载多个 URL，当然有时您希望将这些下载存储在命名良好的本地文件中。

理解这个问题的关键在于，每个下载 URL 都需要自己的“存储指令”。如果没有这个“存储指令”，curl 默认会将数据发送到 stdout。如果您请求了两个 URL，但只告诉 curl 第一个 URL 的保存位置，那么第二个 URL 就会被发送到 stdout。就像这样：

```sh
curl -o one.html http://example.com/1 http://example.com/2
```

“存储指令”的读取和处理顺序与下载 URL 相同，因此它们不必以任何方式紧挨着 URL。您可以首先、最后或与 URL 交织地汇总所有输出选项。您来选择。

这些示例都以相同的方式工作：

```sh
curl -o 1.txt -o 2.txt http://example.com/1 http://example.com/2
curl http://example.com/1 http://example.com/2 -o 1.txt -o 2.txt
curl -o 1.txt http://example.com/1 http://example.com/2 -o 2.txt
curl -o 1.txt http://example.com/1 -o 2.txt http://example.com/2
```

`-O`同样只是一个针对单个下载的指令，因此如果您下载多个 URL，请使用更多的它们：

```sh
curl -O -O http://example.com/1 http://example.com/2
```

## 并行

除非另有说明，curl 会按顺序逐个下载所有给定的 URL。通过使用`-Z`（或`--parallel`），curl 可以改为并行传输传输：一次多个。
