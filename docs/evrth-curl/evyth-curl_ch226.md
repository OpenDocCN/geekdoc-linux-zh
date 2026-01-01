# FTP 通配符匹配

libcurl 支持 FTP 通配符匹配。您通过将`CURLOPT_WILDCARDMATCH`设置为`1L`并然后在 URL 的文件名部分使用“通配符模式”来使用此功能。

## 通配符模式

默认的 libcurl 通配符匹配函数支持：

`*` - 星号

```sh
ftp://example.com/some/path/*.txt
```

要匹配目录`some/path`中的所有 txt 文件。在同一模式字符串中只允许使用两个星号。

`?` - 问号

一个问号匹配任何（确切的一个）字符。例如，如果您有名为`photo1.jpeg`和`photo7.jpeg`的文件，此模式可以匹配它们：

```sh
ftp://example.com/some/path/photo?.jpeg
```

`[` - 括号表达式

左括号开启一个括号表达式。在括号表达式中，问号和星号没有特殊含义。每个括号表达式以右括号（`]`）结束，并匹配一个确切字符。以下是一些示例：

`[a-zA-Z0-9]`或`[f-gF-G]` - 字符区间

`[abc]` - 字符枚举

`[^abc]`或`[!abc]` - 否定

`[[:name:]]` 类表达式。支持的类有 alnum、lower、space、alpha、digit、print、upper、blank、graph、xdigit。

`[][-!^]` - 特殊情况，仅匹配`-`、`]`、`[`、`!`或`^`。

`[\\[\\]\\\\]` - 转义语法。匹配`[`、`]`或`\`。

使用上述规则，可以构建一个文件名模式：

```sh
ftp://example.com/some/path/[a-z[:upper:]\\\\].jpeg
```

## FTP 块回调

当使用 FTP 通配符匹配时，在开始传输匹配的文件之前，会调用`CURLOPT_CHUNK_BGN_FUNCTION`回调函数。

回调函数可以选择返回以下这些返回代码之一，以告诉 libcurl 如何处理该文件：

+   `CURL_CHUNK_BGN_FUNC_OK` 传输文件

+   `CURL_CHUNK_BGN_FUNC_SKIP`

+   `CURL_CHUNK_BGN_FUNC_FAIL` 由于错误而停止

在匹配的文件传输或跳过后，会调用`CURLOPT_CHUNK_END_FUNCTION`回调函数。

结束块回调函数只能返回成功或错误。

## FTP 匹配回调

如果默认的模式匹配函数不符合您的喜好，您可以通过设置`CURLOPT_FNMATCH_FUNCTION`选项为您的替代函数来提供自己的替换函数。
