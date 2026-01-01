# 将下载保存到由 URL 命名的文件中

然而，许多 URL 的右端已经包含了文件名部分。curl 允许你使用这个快捷方式，这样你就不必用`-o`重复它。而不是：

```sh
curl -o file.html http://example.com/file.html
```

你可以用这个命令将远程 URL 资源保存到本地文件`file.html`中：

```sh
curl -O http://example.com/file.html
```

这是`-O`（大写字母 O）选项，或者长名称版本的`--remote-name`。`-O`选项通过选择你提供的 URL 的文件名部分来选择要使用的本地文件名。这很重要。你指定 URL，curl 会从这个数据中选取名称。如果网站重定向 curl 到其他地方（如果你告诉 curl 跟随重定向），它不会改变 curl 用于存储此内容的文件名。

## 为所有 URL 使用 URL 的文件名部分

作为对使用一百个 URL 时添加一百个`-O`选项的反应，我们引入了一个名为`--remote-name-all`的选项。这使得`-O`成为所有给定 URL 的默认操作。你仍然可以为 URL 提供单独的“存储指令”，但如果为下载的 URL 省略了它，那么默认操作将从 stdout 切换到`-O`风格。
