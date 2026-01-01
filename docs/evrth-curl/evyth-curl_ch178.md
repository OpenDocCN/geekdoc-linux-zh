# Cookies

HTTP Cookies 是客户端代表服务器存储的键/值对。它们在后续请求中发送回来，以便客户端能够在请求之间保持状态。记住，HTTP 协议本身没有状态，而是客户端必须在后续请求中重新发送所有它希望服务器知道的数据。

服务器通过`Set-Cookie:`头设置 Cookies，并且每个 Cookies 都会发送一些额外的属性，客户端需要匹配这些属性才能将 Cookies 发送回服务器。这些属性包括域名、路径，也许最重要的是 Cookies 应该存活的时间。

Cookies 的过期时间要么设置为未来的固定时间（或存活一定秒数），要么根本不设置过期时间。没有过期时间的 Cookies 被称为会话 Cookies，意味着它在会话期间存在，但不会更久。在这个意义上，会话通常被认为是用于查看网站的浏览器的生命周期。当你关闭浏览器时，你就结束了会话。使用支持 Cookies 的命令行客户端进行 HTTP 操作时，会引发一个疑问：会话何时真正结束…

## Cookies 引擎

curl 的一般概念是除非你告知它不同，否则只做最基本的工作，这使得它默认不识别 Cookies。你需要开启 Cookies 引擎来让 curl 跟踪它接收到的 Cookies，并在具有匹配 Cookies 的请求中发送它们。

你可以通过让 curl 读取或写入 Cookies 来开启 Cookies 引擎。如果你告诉 curl 从一个空命名的文件中读取 Cookies，你只是开启了引擎，但开始时内部 Cookies 存储是空的：

```sh
curl -b "" http://example.com
```

只开启 Cookies 引擎，获取单个资源然后退出是没有意义的，因为 curl 没有机会发送它接收到的任何 Cookies。假设在这个示例中，网站会设置 Cookies 然后进行重定向，我们会这样做：

```sh
curl -L -b "" http://example.com
```

## 从文件中读取 Cookies

从一个空的 Cookies 存储开始可能不是最佳选择。为什么不从之前获取或以其他方式获得的 Cookies 开始呢？curl 用于 Cookies 的文件格式被称为 Netscape Cookies 格式，因为它曾经是浏览器使用的文件格式，然后你可以轻松地告诉 curl 使用浏览器的 Cookies。

作为便利，curl 还支持将 Cookies 文件作为设置 Cookies 的 HTTP 头集合。这是一个次优格式，但可能是你唯一拥有的。

告诉 curl 从哪个文件中读取初始 Cookies：

```sh
curl -L -b cookies.txt http://example.com
```

记住，这只是为了从文件中读取。如果服务器在其响应中更新 Cookies，curl 会在其内存存储中更新该 Cookies，但最终在退出时会丢弃所有 Cookies。当后续调用相同的输入文件时，会再次使用原始的 Cookies 内容。

## 将 Cookies 写入文件

存储 cookies 的地方有时被称为 cookie jar。当你启用 curl 的 cookie 引擎并且它已经接收到了 cookies，你可以指示 curl 在退出之前将其所有已知的 cookies 写入一个文件，即 cookie jar。重要的是要记住，curl 只在退出时更新输出 cookie jar，而不是在其生命周期内，无论处理给定输入需要多长时间。

你可以使用 `-c` 来指定 cookie jar 的输出：

```sh
curl -c cookie-jar.txt http://example.com
```

`-c` 是将 cookies *写入* 文件的指令，`-b` 是从文件 *读取* cookies 的指令。通常你两者都需要。

当 curl 将 cookies 写入此文件时，它会保存所有已知的 cookies，包括那些会话 cookies（没有指定生存期）。curl 本身没有会话的概念，也不知道会话何时结束，因此除非你告诉它，否则它不会刷新会话 cookies。

## 新的 Cookie 会话

curl 不是通过告诉 curl 何时会话结束来处理会话的，它提供了一个选项，让用户决定何时开始一个新的会话 *开始*。

新的 cookie 会话意味着所有旧的会话 cookies 都会被丢弃。这相当于关闭浏览器并重新启动。

使用 `-j, --junk-session-cookies` 告诉 curl 一个新的 Cookie 会话开始：

```sh
curl -j -b cookies.txt http://example.com/
```

+   Cookie 文件格式
