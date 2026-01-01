# 许多选项和 URL

如上所述，curl 支持数百个命令行选项，它还支持无限数量的 URL。如果你的 shell 或命令行系统支持，你可以传递给 curl 的命令行长度实际上是没有限制的。

curl 首先解析整个命令行，应用命令行选项的愿望，然后逐个（从左到右的顺序）遍历 URL 以执行操作。

对于某些选项（例如告诉 curl 存储传输位置的`-o`或`-O`），你可能希望在命令行上为每个 URL 指定一个选项。

curl 为它在最后一个使用的 URL 上的操作返回一个退出代码。如果你希望 curl 在集合中第一个失败的 URL 上出错退出，请使用`--fail-early`选项。

## 每个给定 URL 一个输出

如果你使用包含两个 URL 的命令行，你必须告诉 curl 如何处理这两个 URL。`-o`和`-O`选项指示 curl 如何保存 URL 的输出，所以你可能希望命令行上的选项数量与 URL 数量相同。

如果命令行上的 URL 数量多于输出选项，那么没有对应输出指令的 URL 内容将被发送到 stdout。

使用`--remote-name-all`标志会自动让 curl 表现得像是为所有没有输出选项的给定 URL 使用了`-O`。

## 每个 URL 单独的选项

在前面的章节中，我们描述了 curl 总是解析整个命令行中的所有选项，并将这些选项应用于它传输的所有 URL。

那只是一个简化：curl 还提供了一个选项（`-:`, `--next`），它会在一组选项和 URL 之间插入一个边界，对于这些 URL 它应用了这些选项。当命令行解析器找到一个`--next`选项时，它会将以下选项应用于下一组 URL。因此，`--next`选项在选项集和 URL 之间充当了一个*分隔符*。你可以使用任意多的`--next`选项。

例如，我们向一个 URL 执行 HTTP GET 请求并跟随重定向，然后向另一个不同的 URL 执行第二个 HTTP POST 请求，最后用对第三个 URL 的 HEAD 请求来结束。所有这些都在一个命令行中完成：

```sh
curl --location http://example.com/1 --next
  --data sendthis http://example.com/2 --next
  --head http://example.com/3
```

如果在命令行上没有使用`--next`选项尝试这样做，将会生成一个非法命令行，因为 curl 会尝试同时组合 POST 和 HEAD：

```sh
Warning: You can only select one HTTP request method! You asked for both
Warning: POST (-d, --data) and HEAD (-I, --head).
```
