# 配置文件

使用多个命令行选项的 Curl 命令可能会变得难以操作。字符数量甚至可能超过您的终端应用程序允许的最大长度。

为了帮助这种情况，curl 允许您在纯文本配置文件中编写命令行选项，并告诉 curl 在适用时从该文件中读取选项。

您还可以使用配置文件将数据分配给变量，并使用函数转换数据，这使得它们非常有用。这将在变量部分中讨论。

下面的一些示例包含多行以提高可读性。反斜杠（`\`）用于指示终端忽略换行符。

## 指定要使用的配置文件

使用`-K`或长形式`--config`选项告诉 curl 从配置文件中读取。

```sh
curl  \
    --config configFile.txt \
    --url https://example.com
```

指定的文件路径相对于您的终端中的当前目录。

您可以为配置文件命名任何您喜欢的名称。在上面的示例中，使用`configFile.txt`是为了简单起见。

## 语法

每行输入一个命令。使用井号符号作为注释：

```sh
# curl config file

# Follow redirects
--location

# Do a HEAD request
--head
```

## 命令行选项

您可以使用短选项和长选项，就像您在命令行中写的那样。

您也可以不使用前导的两个连字符来编写长选项，使其更容易阅读。

```sh
# curl config file

# Follow redirects
location

# Do a HEAD request
head
```

## 参数

带参数的命令行选项必须在选项所在的同一行提供其参数。

```sh
# curl config file

user-agent "Everything-is-an-agent"
```

您也可以在选项和其参数之间使用`=`或`:`。如您所见，这不是必需的，但有些人喜欢它提供的清晰度。再次设置用户代理选项：

```sh
# curl config file

user-agent = "Everything-is-an-agent"
```

我们上面使用的用户代理字符串示例没有空格，所以从技术上讲，引号不是必需的：

```sh
# curl config file

user-agent = Everything-is-an-agent
```

有关何时使用引号的更多信息，请参阅下文“何时使用引号”。

## URL

当在命令行中输入 URL 时，所有不是选项的内容都被假定为 URL。然而，在配置文件中，您必须使用`--url`或`url`来指定 URL。

```sh
# curl config file

url = https://example.com
```

## 何时使用引号

您需要在以下情况下使用双引号：

+   参数包含空格，或以字符`:`或`=`开头。

+   您需要使用转义序列（可用选项：`\\`，`\"`，`\t`，`\n`，`\r`和`\v`。任何字母前的反斜杠都被忽略）。

如果包含空格的参数没有用双引号括起来，curl 会认为下一个空格或换行符是参数的结束。

## 默认配置文件

当调用 curl 时，它总是（除非使用`-q`），检查默认配置文件，如果找到则使用它。

Curl 按以下顺序在以下位置查找默认配置文件：

1.  `$CURL_HOME/.curlrc`

1.  `$XDG_CONFIG_HOME/.curlrc`（自 7.73.0 版添加）

1.  `$HOME/.curlrc`

1.  Windows: `%USERPROFILE%\\.curlrc`

1.  Windows: `%APPDATA%\\.curlrc`

1.  Windows: `%USERPROFILE%\\Application Data\\.curlrc`

1.  非 Windows：使用 getpwuid 查找主目录

1.  在 Windows 上，如果在上述序列中找不到`.curlrc`文件，它会在 curl 可执行文件所在的同一目录中查找一个。

在 Windows 系统中，每个位置会检查两个文件名：`.curlrc` 和 `_curlrc`，优先选择前者。旧版的 curl 在 Windows 上仅检查 `_curlrc`。
