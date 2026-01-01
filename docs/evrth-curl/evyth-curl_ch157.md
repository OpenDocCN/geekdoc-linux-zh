# 发布二进制

当从文件读取数据以发布时，`-d` 会去除回车和换行符。如果你想让 curl 读取并使用给定的文件作为二进制文件，则使用 `--data-binary`：

```sh
curl --data-binary @filename http://example.com/
```
