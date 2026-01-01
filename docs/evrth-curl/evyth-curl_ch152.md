# 条件性

有时用户希望避免在文件可能前一天已经下载的情况下再次下载文件。这可以通过使 HTTP 传输条件化来实现。curl 支持两种不同的条件：文件时间戳和 etag。

## 通过修改日期进行检查

使用 `-z` 或 `--time-cond` 选项仅下载比特定日期更新的文件：

```sh
curl -z "Jan 10, 2017" https://example.com/file -O
```

或者相反，如果文件比特定时间旧，可以通过在日期前加一个连字符来获取文件：

```sh
curl --time-cond "-Jan 10, 2017" https://example.com/file -O
```

日期解析器很宽松，接受大多数你可以用日期写出的格式，你也可以指定它包括时间：

```sh
curl --time-cond "Sun, 12 Sep 2004 15:05:58 -0700" \
  https://www.example.org/file.html
```

`-z` 选项还可以从本地文件中提取并使用时间戳，这对于仅在文件被远程更新时下载文件来说很方便：

```sh
curl -z file.html https://example.com/file.html -O
```

通常将 `-z` 与 `--remote-time` 标志结合使用非常有用，该标志将本地创建的文件的时间设置为与远程文件相同的时戳：

```sh
curl -z file.html -o file.html --remote-time https://example.com/file.html
```

## 通过内容修改进行检查

HTTP 服务器可以为给定资源版本返回一个特定的 *ETag*。如果给定 URL 的资源发生变化，必须生成新的 Etag 值，以便客户端知道只要 ETag 保持不变，内容就没有发生变化。

使用 ETags，curl 可以在无需依赖时间或文件日期的情况下检查远程更新。它还使得检查能够检测到亚秒级的变化，这是基于时间戳的检查所无法做到的。

使用 curl 可以下载远程文件并将其 ETag（如果有的话）保存在单独的缓存中，通过使用 `--etag-save` 命令行选项。例如：

```sh
curl --etag-save etags.txt https://example.com/file -o output
```

随后的命令行可以使用之前保存的 etag，并确保只有在文件发生变化时才再次下载文件，如下所示：

```sh
curl --etag-compare etag.txt https://example.com/file -o output
```

这两个 etag 选项也可以在同一个命令行中结合使用，这样如果文件实际上已经更新，curl 会再次将更新 ETag 保存到文件中：

```sh
curl --etag-compare etag.txt --etag-save etag.txt \
  https://example.com/file -o output
```
