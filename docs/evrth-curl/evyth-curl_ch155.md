# 简单的 POST

要发送表单数据，浏览器会将它编码为一系列由和号（`&`）符号分隔的`name=value`对。生成的字符串作为 POST 请求的主体发送。要在 curl 中执行相同的操作，使用`-d`（或`--data`）参数，如下所示：

```sh
curl -d 'name=admin&shoesize=12' http://example.com/
```

当在命令行上指定多个`-d`选项时，curl 会将它们连接起来，并在它们之间插入和号，因此上述示例也可以这样写：

```sh
curl -d name=admin -d shoesize=12 http://example.com/
```

如果要发送的数据量太大，无法在命令行上的简单字符串中处理，您也可以按照标准 curl 风格从文件名中读取它：

```sh
curl -d @filename http://example.com
```

虽然服务器可能假设数据是以某种特殊方式编码的，但 curl 不会对您告诉它发送的数据进行编码或更改。**curl 发送您给出的确切字节**（除了从文件读取时。`-d`会跳过回车符和换行符，因此如果您希望它们包含在数据中，则需要使用`--data-binary`）。

要发送以`@`符号开始的 POST 主体，以避免 curl 尝试将其作为文件名加载，请使用`--data-raw`。此选项没有文件加载功能：

```sh
curl --data-raw '@string' https://example.com
```
