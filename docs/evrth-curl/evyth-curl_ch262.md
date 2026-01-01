# 解析 URL

通过在句柄中*设置* `CURLUPART_URL`部分来解析完整 URL：

```sh
CURLU *h = curl_url();
rc = curl_url_set(h, CURLUPART_URL,
                  "https://example.com:449/foo/bar?name=moo", 0);
```

如果成功，`rc`包含`CURLUE_OK`，并且不同的 URL 组件保留在句柄中。这意味着从 libcurl 的角度来看，URL 是有效的。

函数调用的第四个参数是一个掩码。设置该掩码中的零位、一位或多位以改变解析器的行为：

## `CURLU_NON_SUPPORT_SCHEME`

使`curl_url_set()`接受非支持的方案。如果没有设置，则唯一可接受的方案是 libcurl 知道并具有内置支持的协议。

## `CURLU_URLENCODE`

如果路径中的任何字节从编码中受益，则使函数对路径部分进行 URL 编码：如空格或“控制字符”。

## `CURLU_DEFAULT_SCHEME`

如果传入的字符串未使用方案，则假定默认方案被意图使用。默认方案是 HTTPS。如果未设置，则不接受没有方案部分的 URL 作为有效 URL。如果两者都设置了，则覆盖`CURLU_GUESS_SCHEME`选项。

## `CURLU_GUESS_SCHEME`

使 libcurl 允许设置没有方案的 URL，并且它根据主机名“猜测”所意图的方案。如果最外层的子域名与 DICT、FTP、IMAP、LDAP、POP3 或 SMTP 匹配，则使用该方案，否则选择 HTTP。与`CURLU_DEFAULT_SCHEME`选项冲突，如果两者都设置了，则优先级更高。

## `CURLU_NO_AUTHORITY`

跳过权限检查。RFC 允许个别方案省略主机部分（通常是权限的唯一必需部分），但 libcurl 无法知道这是否允许自定义方案。指定该标志允许空权限部分，类似于处理文件方案的方式。实际上，仅在与`CURLU_NON_SUPPORT_SCHEME`结合使用时才可用。

## `CURLU_PATH_AS_IS`

使 libcurl 跳过路径的正常化。这是 curl 通常删除点斜杠序列（如点点和点点等）的程序。用于传输的相同选项称为`CURLOPT_PATH_AS_IS`。

## `CURLU_ALLOW_SPACE`

使 URL 解析器尽可能允许空格（ASCII 32）。URL 语法通常不允许任何地方有空格，但它们应编码为`%20`或`+`。当允许空格时，它们在方案中仍然不允许。当在 URL 中使用并允许空格时，除非也设置了`CURLU_URLENCODE`，否则将按原样存储。这会影响随后使用`curl_url_get()`提取完整 URL 或单个部分时 URL 的构建方式。
