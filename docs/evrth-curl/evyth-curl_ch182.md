# 脚本化类似浏览器的任务

curl 可以执行几乎所有的 HTTP 操作，并且可以像你最喜欢的浏览器一样传输。实际上，它还可以做更多的事情，但在这个章节中，我们关注的是你可以使用 curl 来重现或脚本化你否则必须手动使用浏览器执行的操作。

这里有一些技巧和建议，关于在执行此操作时如何进行。

## 确定浏览器做了什么

这实际上是一个必要的第一步。猜测它做了什么可能会让你陷入错误的问题陷阱。对这个问题的科学方法几乎要求你首先了解浏览器做了什么。

要了解浏览器如何执行特定任务，你可以阅读你操作的 HTML 页面，如果你有足够的知识，你可以看到浏览器为了完成它会如何操作，然后开始尝试用 curl 做同样的事情。

一种稍微有效的方法，即使在页面充满了混淆的 JavaScript 的情况下也能工作，是运行浏览器并监控它执行的 HTTP 操作。

复制为 curl 部分描述了如何记录浏览器的请求并将其轻松转换为 curl 命令行。

虽然复制 curl 命令行通常还不够好，因为它们倾向于**精确地**复制那个请求，而你可能更希望它更加动态，以便你可以重现相同的操作，而不仅仅是重新发送原始请求。

## Cookies

当今许多网站都使用某个地方的用户名和密码登录提示。在许多情况下，你甚至很久以前就已经用浏览器登录过了，但它保持了状态，并保持你登录的状态。

登录状态几乎总是通过使用 cookies 来完成的。一个常见的操作是首先登录并将返回的 cookies 保存到文件中，然后当使用 curl 遍历网站时，让网站在后续的命令行中更新 cookies。

## 网络登录和会话

网址为 https://example.com/的网站有一个登录提示。该网站的登录是一个 HTML 表单，你向其发送一个 HTTP POST 请求。保存响应 cookies 和响应（HTML）输出。

尽管登录页面在 https://example.com/上是可见的（如果你使用浏览器），但该页面的 HTML 表单标签会告诉你应该将 POST 发送到哪个确切的 URL，使用`action`参数。

在我们的假设案例中，表单标签看起来像这样：

```sh
<form action="login.cgi" method="POST">
  <input type="text" name="user">
  <input type="password" name="secret">
  <input type="hidden" name="id" value="bc76">
</form>
```

有三个重要的字段。**text**、**secret**和**id**。最后一个，即 id，被标记为`hidden`，这意味着它不会在浏览器中显示，并且它不是一个用户需要填写的字段。它由网站本身生成，为了你的 curl 登录成功，你需要提取该值，并在 POST 提交中使用该值以及其余数据。

将正确的内容发送到正确的目标 URL 的字段：

```sh
curl -d user=daniel -d secret=qwerty -d id=bc76 \
  https://example.com/login.cgi -o out
```

许多登录页面甚至在你展示登录时就已经发送了会话 cookie，而且由于你通常需要从`<form>`标签中提取隐藏字段，你可以先这样做：

```sh
curl -c cookies https://example.com/ -o loginform
```

你通常会需要一个 HTML 解析器或某种脚本语言来从那里提取 id 字段，然后你可以继续按照上述方法登录，但增加了 cookie 加载（我将这一行分成两行以提高可读性）：

```sh
curl -d user=daniel -d secret=qwerty -d id=bc76 \
  https://example.com/login.cgi -b cookies -c cookies -o out
```

你可以看到它同时使用了`-b`来从文件中读取 cookies，以及`-c`来存储 cookies，以便在服务器发送回更新的 cookies 时使用。

总是，*总是*，在处理细节时添加`-v`到命令行。有关更多详细信息，请参阅详细输出部分。

## 重定向

服务器在响应登录 POST 请求时使用重定向是很常见的。如此常见，以至于我可能会说，如果不通过重定向解决，那是非常罕见的。

然后，你只需记住 curl 不会自动跟随重定向。你需要通过添加`-L`命令行选项来指示它这样做。将此添加到之前的命令行后，完整的命令行看起来像这样：

```sh
curl -d user=daniel -d secret=qwerty -d id=bc76 \
  https://example.com/login.cgi -b cookies -c cookies -L -o out
```

## 登录后

在上述示例命令行中，我们将登录响应输出保存到名为‘out’的文件中，在你的脚本中，你可能需要验证它包含一些文本或确认登录成功的某些内容。

一旦成功登录，获取所需的文件或执行 HTTP 操作，并记得在命令行上继续使用`-b`和`-c`来使用和更新 cookies。

## 引用者

一些网站在请求某些内容或登录或类似操作时，会验证`Referer:`是否确实标识了合法的父 URL。然后，你可以通过使用`-e https://example.com/`等来通知服务器你从哪个 URL 到达的。将此添加到之前的登录尝试后，它看起来像这样：

```sh
curl -d user=daniel -d secret=qwerty -d id=bc76 \
  https://example.com/login.cgi \
  -b cookies -c cookies -L -e "https://example.com/" -o out
```

## TLS 指纹识别

现今，反机器人检测通常使用 TLS 指纹识别来确定请求是否来自浏览器。Curl 的指纹可能会根据你的环境而变化，并且很可能是与浏览器不同的。Curl 的 CLI 没有选项来更改指纹的各个部分，但是高级用户可以通过使用 libcurl 并自行编译 curl 来自定义指纹。
