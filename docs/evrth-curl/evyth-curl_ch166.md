# 多部分表单

多部分表单是当 HTML 表单提交时，`enctype`设置为`multipart/form-data`时，HTTP 客户端发送的内容。这是一个带有特殊格式化的请求体，作为一系列部分发送的 HTTP POST 请求，这些部分由 MIME 边界分隔。

一个 HTML 示例可能如下所示：

```sh
<form action="submit.cgi" method="post" enctype="multipart/form-data">
  Name: <input type="text" name="person"><br>
  File: <input type="file" name="secret"><br>
  <input type="submit" value="Submit">
</form>
```

在网页浏览器中，它可能看起来像这样：

![](img/file12.png)

一个多部分表单

用户可以在“名称”字段中填写文本，通过按下`浏览`按钮选择一个本地文件，当按下`提交`按钮时，该文件将被上传。

## 使用 curl 发送此类表单

使用 curl，你可以通过一个`-F`（或`--form`）标志添加每个单独的多部分，然后继续添加一个`-F`标志，为表单中你想要发送的每个输入字段添加一个。

上述简短示例表单有两个部分，一个名为“person”的纯文本字段和一个名为“secret”的文件。

按照如下方式将数据发送到该表单：

```sh
curl -F person=anonymous -F secret=@file.txt http://example.com/submit.cgi
```

## 生成的 HTTP 请求

**动作**指定了 POST 请求发送的位置。**方法**说明这是一个 POST 请求，而**编码类型**告诉我们这是一个多部分表单。

如上所示，curl 根据填写的字段生成并发送这些 HTTP 请求头到主机 example.com：

```sh
POST /submit.cgi HTTP/1.1
Host: example.com
User-Agent: curl/7.46.0
Accept: */*
Content-Length: 313
Expect: 100-continue
Content-Type: multipart/form-data; boundary=------------d74496d66958873e
```

**内容长度**当然告诉服务器期望多少数据。这个示例的 313 字节非常小。

**Expect**头信息在 Expect 100 continue 章节中有解释。

**内容类型**头信息有些特殊。它表明这是一个多部分表单，然后设置边界字符串。边界字符串是一行字符，其中包含一些随机的数字，用作提交表单的不同部分之间的分隔符。在这个示例中，你看到的特定边界包含随机部分`d74496d66958873e`，但当你运行 curl（或使用浏览器提交此类表单时），你当然会得到不同的结果。

因此，在初始的一组头信息之后是请求体。

```sh
--------------------------d74496d66958873e
Content-Disposition: form-data; name="person"

anonymous
--------------------------d74496d66958873e
Content-Disposition: form-data; name="secret"; filename="file.txt"
Content-Type: text/plain

contents of the file
--------------------------d74496d66958873e--
```

在这里，你可以清楚地看到发送的两个部分，它们由边界字符串分隔。每个部分都以一个或多个头信息开始，描述了该部分的内容，包括其名称和可能的一些其他细节。然后在该部分的头信息之后是实际的数据，没有任何编码。

最后的边界字符串附加了两个额外的破折号`--`，以表示结束。

## 内容类型

使用 curl 的`-F`选项会使其请求包含一个默认的`Content-Type`头信息，如上述示例所示。这表明`multipart/form-data`，然后指定 MIME 边界字符串。这个`Content-Type`是多部分表单的默认值，但你当然可以修改它以适应自己的命令。如果你这样做，curl 足够聪明，仍然会在替换的头信息中附加边界魔法。你实际上无法更改边界字符串，因为 curl 需要它来生成 POST 流。

要替换头信息，使用`-H`选项，如下所示：

```sh
curl -F 'name=Dan' -H 'Content-Type: multipart/magic' https://example.com
```

## 转换网页表单

有几种不同的方法可以确定如何编写一个 curl 命令行来提交一个如 HTML 中所见的多部分表单。

1.  将 HTML 本地保存，在本地运行 `nc` 监听所选的端口号，将 `action` URL 更改为提交 POST 到你的本地 `nc` 实例。提交表单并观察 `nc` 如何显示它。然后将其转换为 curl 命令行。

1.  使用你最喜欢的浏览器的开发工具，在网络标签中检查你提交后的 POST 请求。然后将那些 HTTP 数据转换为 curl 命令行。不幸的是，浏览器中的 复制为 curl 功能通常并不特别擅长处理多部分表单的 POST。

1.  检查源 HTML 并直接将其转换为 curl 命令行。

## 从 `<form>` 到 -F

在使用 `enctype="multipart/form-data"` 的 `<form>` 中，第一步是找到 `action=` 属性，因为它告诉 POST 的目标。你需要将其转换为 curl 命令行的完整 URL。

一个示例动作看起来像这样：

```sh
<form action="submit.cgi" method="post" enctype="multipart/form-data">
```

如果表单位于一个 URL 如 `https://example.com/user/login` 的网页上，那么 `action=submit.cgi` 是表单本身所在目录内的相对路径。因此，提交此表单的完整 URL 变为 `https://example.com/user/submit.cgi`。这就是 curl 命令行中要使用的 URL。

接下来，你必须识别表单中使用的每个 `<input>` 标签，包括标记为隐藏的标签。隐藏只是意味着它们不会显示在网页上，但它们应该仍然在 POST 中发送。

表单中的每个 `<input>` 都应该在命令行中有一个相应的 `-F`。

### 文本输入

一个使用类似文本类型的常规标签

```sh
<input type="text" name="person">
```

应设置字段名称，内容如下：

```sh
curl -F "person=Mr Smith" https://example.com/
```

### 文件输入

当输入类型设置为文件时，如：

```sh
<input type="file" name="image">
```

你通过指定文件名来提供这部分文件，并使用 `@` 和文件的路径来包含：

```sh
curl -F image=@funnycat.gif https://example.com/
```

### 隐藏输入

将状态从表单传递过去的一个常见技术是将多个 `<input>` 标签设置为 `type="hidden"`。这基本上与已经填写好的表单字段相同，所以你可以通过使用名称和值将其转换为命令行。例如：

```sh
<input type="hidden" name="username" value="bob123">
```

这与正常文本字段的转换方式相同，并且你知道内容应该是什么：

```sh
curl -F "username=bob123" https://example.com/
```

### 所有字段同时

如果我们假设上面示例中显示的所有三个不同的 `<input>` 标签都在同一个 `<form>` 中使用，那么一个完整的 curl 命令行，包括上面提取的正确 URL，将看起来像这样：

```sh
curl -F "person=Mr Smith" -F image=@funnycat.gif -F "username=bob123" \
  https://example.com/user/submit.cgi
```
