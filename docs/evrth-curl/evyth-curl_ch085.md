# 存储下载

如果你尝试上一节中的示例下载，你可能会注意到 curl 将下载的数据输出到 stdout，除非你告诉它做其他事情。将数据输出到 stdout 在你想将其管道传输到另一个程序或类似程序时非常有用，但并不总是处理下载的最佳方式。

使用 `-o [filename]`（将 `--output` 作为选项的长版本）给 curl 提供一个特定的文件名来保存下载，其中 filename 可以是文件名，到文件名的相对路径或文件的完整路径。

还要注意，你可以将 `-o` 放在 URL 之前或之后；这没有区别：

```sh
curl -o output.html http://example.com/
curl -o /tmp/index.html http://example.com/
curl http://example.com -o ../../folder/savethis.html
```

当然，这不仅仅限于 `http://` URL，无论你下载哪种类型的 URL 都以相同的方式工作：

```sh
curl -o file.txt ftp://example.com/path/to/file-name.ext
```

如果你要求 curl 将输出发送到终端，它会尝试检测并阻止二进制数据被发送到那里，因为这样会严重破坏你的终端（有时甚至会导致其停止工作）。你可以使用 `-o -` 覆盖 curl 的二进制输出预防并强制输出被发送到 stdout。

curl 有几种其他方式来存储和命名下载的数据。详细信息如下。

## 覆盖写入

当 curl 将远程资源下载到本地文件名时，如上所述，如果该文件已存在，它会覆盖该文件。它会 *覆盖* 它。

curl 提供了一种避免这种覆盖的方法：`--no-clobber`。

当使用此选项时，如果 curl 发现已存在具有给定名称的文件，它会尝试在文件名中添加一个点和一个数字来找到一个尚未使用的名称。它从 `1` 开始，然后继续尝试数字，直到达到 `100` 并选择第一个可用的一个。

例如，如果你要求 curl 下载一个 URL 到 `picture.png`，并且在该目录中已经有两个名为 `picture.png` 和 `picture.png.1` 的文件，以下将文件保存为 `picture.png.2`：

```sh
curl --no-clobber https://example.com/image -o picture.png
```

用户可以使用 –write-out 选项的 `%filename_effective` 变量来确定最终使用的名称。

## 错误时的残留

默认情况下，如果 curl 在下载过程中遇到问题并以错误退出，部分传输的文件将保持原样。它可能是预期文件的一小部分，也可能是几乎整个文件。用户必须决定如何处理残留文件。

`--remove-on-error` 命令行选项改变了这种行为。它告诉 curl 在 curl 因为错误退出时删除任何部分保存的文件。不再有残留文件。
