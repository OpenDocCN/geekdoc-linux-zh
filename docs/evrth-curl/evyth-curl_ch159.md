# URL 编码数据

百分比编码，也称为 URL 编码，在技术上是一种编码数据的方式，以便它可以在 URL 中出现。这种编码通常用于发送 `application/x-www-form-urlencoded` 内容类型的 POST，例如 curl 通过 `--data` 和 `--data-binary` 等发送的。

上文提到的命令行选项都需要你提供正确编码的数据，你需要确保这些数据已经以正确的格式存在。虽然这给了你很大的自由度，但在某些时候也会有些不便。

为了帮助你发送尚未编码的数据，curl 提供了 `--data-urlencode` 选项。此选项提供了多种方法来对给定的数据进行 URL 编码。

你可以使用与其它 –data 选项相同的风格使用它，例如 `--data-urlencode data`。为了符合 CGI 标准，**data** 部分应该以一个名称开头，后面跟着一个分隔符和一个内容指定。**data** 部分可以通过以下语法之一传递给 curl：

+   `content`：对内容进行 URL 编码并传递。只需小心确保内容中不包含任何 `=` 或 `@` 符号，因为这会使语法与以下其他情况之一匹配。

+   `=content`：对内容进行 URL 编码并传递。初始的 `=` 符号不包括在数据中。

+   `name=content`：对内容部分进行 URL 编码并传递。请注意，预期名称部分已经进行了 URL 编码。

+   `@filename`：从指定的文件中加载数据（包括任何换行符），对数据进行 URL 编码，并在 POST 中传递。

+   `name@filename`：从指定的文件中加载数据（包括任何换行符），对数据进行 URL 编码，并在 POST 中传递。名称部分会附加一个等号，结果为 `name=urlencoded-file-content`。请注意，预期名称已经进行了 URL 编码。

例如，你可以通过 POST 发送一个名称，让 curl 进行编码：

```sh
curl --data-urlencode "name=John Doe (Junior)" http://example.com
```

…这将实际请求体中发送以下数据：

```sh
name=John%20Doe%20%28Junior%29
```

如果你将字符串 `John Doe (Junior)` 存储在名为 `contents.txt` 的文件中，你可以告诉 curl 使用字段名 ‘user’ 以这种方式发送该内容 URL 编码：

```sh
curl --data-urlencode user@contents.txt http://example.com
```

在上述两个示例中，字段名没有进行 URL 编码，而是直接传递。如果你想对字段名也进行 URL 编码，比如你想传递一个名为 `user name` 的字段名，你可以要求 curl 对整个字符串进行编码，通过在前面加上一个等号（该等号不会被发送）：

```sh
curl --data-urlencode "=user name=John Doe (Junior)" http://example.com
```
