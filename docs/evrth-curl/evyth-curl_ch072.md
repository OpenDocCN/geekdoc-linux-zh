# 变量

这个命令行和配置文件变量的概念是在 curl 8.3.0 中添加的。

用户可以使用 `--variable varName=content` 将一个 *变量* 设置为纯字符串，或者从文件内容中设置，使用 `--variable varName@file`，其中文件可以是如果设置为单个短横线（`-`）的 stdin。

在此上下文中，变量被赋予一个特定的名称并包含内容。可以设置任意数量的变量。如果您再次设置相同的变量名称，它将被新的内容覆盖。变量名称区分大小写，最长可达 128 个字符，可以由字符 a-z、A-Z、0-9 和下划线组成。

以下一些示例包含多行以提高可读性。反斜杠（`\`）用于指示终端忽略换行。

## 设置变量

您可以使用 `--variable` 在命令行中设置变量，或者使用配置文件中的 `variable`（不带短横线）设置变量。

```sh
curl --variable varName=content
```

或者在一个配置文件中：

```sh
# Curl config file

variable varName=content
```

## 从文件分配内容

您也可以将纯文本文件的内容分配给变量：

```sh
curl --variable varName@filename
```

## 展开

当选项名称以 `--expand-` 为前缀时，可以使用 `{{varName}}` 在选项参数中展开变量。这使得变量 `varName` 的内容被插入。

如果您引用的名称作为变量不存在，则插入一个空字符串。

通过转义符将 `{{` 原样插入字符串中：

`\\{{`

在下面的示例中，设置了变量 `host` 并进行了展开：

```sh
curl \ 
    --variable host=example \
    --expand-url "https://{{host}}.com"
```

对于没有 `--expand-` 前缀指定的选项，变量不会被展开。

变量内容包含在展开时未编码的空字节会导致 curl 以错误退出。

## 环境变量

使用 `--variable %VARNAME` 导入环境变量。如果给定的环境变量未设置，此导入将使 curl 以错误退出。用户还可以选择如果环境变量不存在，则使用 `=content` 或 `@file` 设置默认值，如上所述。

例如，将 `%USER` 环境变量分配给 curl 变量并将其插入到 URL 中。因为没有指定默认值，所以如果环境变量不存在，此操作将失败：

```sh
curl \ 
    --variable %USER \
    --expand-url "https://example.com/api/{{USER}}/method"
```

相反，如果 `%USER` 不存在，则使用 `dummy` 作为默认值：

```sh
curl \
    --variable %USER=dummy \
    --expand-url "https://example.com/api/{{USER}}/method"
```

## 展开 `--variable`

`--variable` 选项本身也可以展开，这允许您将变量分配给其他变量的内容。

```sh
curl \
    --expand-variable var1={{var2}} \
    --expand-variable fullname=’Mrs {{first}} {{last}}’ \
    --expand-variable source@{{filename}}
```

或者在一个配置文件中：

```sh
# Curl config file

variable host=example

expand-variable url=https://{{host}}.com

expand-variable source@{{filename}}
```

## 函数

当展开变量时，curl 提供一组 *函数* 来更改它们的展开方式。函数在变量后使用冒号加函数名应用，如下所示：`{{varName:function}}`。

可以将多个函数应用于变量。它们将按从左到右的顺序应用：`{{varName:func1:func2:func3}}`

这些功能可用：`trim`、`json`、`url` 和 `b64`

## 函数：`trim`

展开变量时，不包含前导和尾随空白。空白定义为：

+   水平制表符

+   空格

+   新行

+   垂直标签

+   表格分隔符和回车符

这在从文件读取数据时非常有用。

```sh
--expand-url “https://example.com/{{path:trim}}”
```

## 函数：`json`

将变量扩展为有效的 JSON 字符串。这使得将有效的 JSON 插入参数中变得更容易（结果 JSON 中不包括引号）。

```sh
--expand-json "\"full name\": \"{{first:json}} {{last:json}}\""
```

要先修剪变量，请按以下顺序应用这两个函数：

```sh
--expand-json "\"full name\": \"{{varName:trim:json}}\""
```

## 函数：`url`

扩展变量为 URL 编码。也称为 *百分编码*。此函数确保所有输出字符在 URL 中都是合法的，其余字符编码为 `%HH`，其中 `HH` 是表示 ascii 值的两个十六进制数字。

```sh
--expand-data “varName={{varName:url}}”
```

要先修剪变量，请按以下顺序应用这两个函数：

```sh
--expand-data “varName={{varName:trim:url}}”
```

## 函数：`b64`

将变量扩展为 base64 编码。Base64 是一种仅使用 64 个特定字符的二进制数据编码方式。

```sh
--expand-data “content={{value:b64}}”
```

要先修剪变量，请按以下顺序应用这两个函数：

```sh
--expand-data “content={{value:trim:b64}}”
```

示例：将名为 `$HOME/.secret` 的文件内容获取到名为 `fix` 的变量中。确保内容经过修剪并作为 POST 数据进行百分编码发送：

```sh
curl \
    --variable %HOME=/home/default \
    --expand-variable fix@{{HOME}}/.secret \
    --expand-data "{{fix:trim:url}}" \
    --url https://example.com/ \
```
