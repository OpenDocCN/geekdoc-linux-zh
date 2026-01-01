# Cookies

默认情况下，libcurl 将传输设置为尽可能简单，需要启用功能才能使用。其中一个功能就是 HTTP cookie，更通俗地称为 cookie。

Cookies 是由服务器（使用`Set-Cookie:`头）发送的名称/值对，用于存储在客户端，并且应该在请求中再次发送，以匹配服务器（使用`Cookie:`头）指定的主机和路径要求。在今天的现代网络中，网站有时会使用大量的 cookie。

## Cookie 引擎

当您为特定的简单句柄启用 cookie 引擎时，这意味着它记录传入的 cookie，将它们存储在与简单句柄关联的内存中 cookie 存储中，并在发出匹配的 HTTP 请求时发送适当的 cookie。

有两种方法可以开启 cookie 引擎：

### 使用读取启用 cookie 引擎

使用`CURLOPT_COOKIEFILE`选项请求 libcurl 从给定的文件名导入 cookie：

```sh
curl_easy_setopt(easy, CURLOPT_COOKIEFILE, "cookies.txt");
```

一个常见的技巧是只指定一个不存在的文件名或空字符串`""`，以便它只使用空白的 cookie 存储来启动 cookie 引擎。

此选项可以设置多次，然后读取每个给定的文件。

### 使用写入启用 cookie 引擎

使用`CURLOPT_COOKIEJAR`选项请求将接收到的 cookie 存储在文件中：

```sh
curl_easy_setopt(easy, CURLOPT_COOKIEJAR, "cookies.txt");
```

当稍后使用`curl_easy_cleanup()`关闭简单句柄时，所有已知的 cookie 都会存储在给定的文件中。文件格式是众所周知的 Netscape cookie 文件格式，浏览器也曾使用过。

## 设置自定义 cookie

一个更简单、更直接的方法是只需在请求中传递一组特定的 cookie，这不会向 cookie 存储添加任何 cookie，甚至不会激活 cookie 引擎，只需使用`CURLOPT_COOKIE`设置即可：

```sh
curl_easy_setopt(easy, CURLOPT_COOKIE, "name=daniel; present=yes;");
```

您设置的字符串是将在 HTTP 请求中发送的原始字符串，应该格式为重复的`NAME=VALUE;`序列 - 包括分号分隔符。

## 导入导出

内存中的 cookie 存储可以保存大量 cookie，libcurl 为应用程序提供了非常强大的方式来操作它们。您可以设置新的 cookie，可以替换现有的 cookie，也可以提取现有的 cookie。

### 向 cookie 存储中添加 cookie

通过将新的 cookie 传递给 curl 并使用`CURLOPT_COOKIELIST`选项，可以简单地向 cookie 存储中添加新的 cookie。输入格式是 cookie 文件格式中的一行，或者格式化为`Set-Cookie:`响应头，但我们推荐使用 cookie 文件风格：

```sh
#define SEP  "\t"  /* Tab separates the fields */

char *my_cookie =
  "example.com"    /* Hostname */
  SEP "FALSE"      /* Include subdomains */
  SEP "/"          /* Path */
  SEP "FALSE"      /* Secure */
  SEP "0"          /* Expiry in epoch time format. 0 == Session */
  SEP "foo"        /* Name */
  SEP "bar";       /* Value */

curl_easy_setopt(curl, CURLOPT_COOKIELIST, my_cookie);
```

如果给定的 cookie 与已存在的 cookie（具有相同的域名和路径等）匹配，它将用新内容覆盖旧的 cookie。

### 从 cookie 存储中获取所有 cookie

有时在关闭句柄时写入 cookie 文件是不够的，然后您的应用程序可以选择像这样从存储中提取所有当前已知的 cookie：

```sh
struct curl_slist *cookies
curl_easy_getinfo(easy, CURLINFO_COOKIELIST, &cookies);
```

这返回一个指向 cookie 链表的指针，每个 cookie 都（再次）指定为 cookie 文件格式的一行。列表为您分配，因此当应用程序完成信息处理时，请务必调用 `curl_slist_free_all`。

### Cookie 存储命令

如果设置和提取 cookie 不够，您还可以以更多方式干扰 cookie 存储：

使用以下命令清除整个内存存储：

```sh
curl_easy_setopt(curl, CURLOPT_COOKIELIST, "ALL");
```

从内存中擦除所有会话 cookie（没有过期日期的 cookie）：

```sh
curl_easy_setopt(curl, CURLOPT_COOKIELIST, "SESS");
```

强制将所有 cookie 写入之前通过 `CURLOPT_COOKIEJAR` 指定的文件名：

```sh
curl_easy_setopt(curl, CURLOPT_COOKIELIST, "FLUSH");
```

强制从之前通过 `CURLOPT_COOKIEFILE` 指定的文件名重新加载 cookie：

```sh
curl_easy_setopt(curl, CURLOPT_COOKIELIST, "RELOAD");
```

## Cookie 文件格式

Cookie 文件格式是基于文本的，每行存储一个 cookie。以 `#` 开头的行被视为注释。

每行指定一个单个 cookie，由制表符字符分隔的七个文本字段组成。

| 字段 | 示例 | 含义 |
| --- | --- | --- |
| 0 | example.com | 域名 |
| 1 | FALSE | 包含子域布尔值 |
| 2 | /foobar/ | 路径 |
| 3 | FALSE | 通过安全传输设置 |
| 4 | 1462299217 | 过期时间 – 自 1970 年 1 月 1 日起的秒数，或 0 |
| 5 | person | Cookie 的名称 |
| 6 | daniel | Cookie 的值 |
