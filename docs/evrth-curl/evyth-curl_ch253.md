# 上传

通过 HTTP 进行上传可以以许多不同的方式完成，并且注意这些差异非常重要。它们可以使用不同的方法，如 POST 或 PUT，并且在使用 POST 时，主体格式可能不同。

除了这些 HTTP 差异之外，libcurl 还提供了不同的方式来提供上传的数据。

## HTTP POST

POST 通常是传递数据到远程 Web 应用的 HTTP 方法。在浏览器中，通常通过填写 HTML 表单并按提交按钮来实现这一点。这是 HTTP 请求传递数据到服务器的标准方式。使用 libcurl，你通常提供数据作为指针和长度：

```sh
curl_easy_setopt(easy, CURLOPT_POSTFIELDS, dataptr);
curl_easy_setopt(easy, CURLOPT_POSTFIELDSIZE, (long)datalength);
```

或者，你可以告诉 libcurl 这是一个 POST 操作，但更希望它通过使用常规读取回调来获取数据：

```sh
curl_easy_setopt(easy, CURLOPT_POST, 1L);
curl_easy_setopt(easy, CURLOPT_READFUNCTION, read_callback);
```

这个“正常”的 POST 也设置了请求头`Content-Type: application/x-www-form-urlencoded`。

## HTTP 多部分表单

多部分表单仍然使用相同的 HTTP 方法 POST；区别仅在于请求体的格式。多部分表单是一系列分开的“部分”，由 MIME 风格的边界字符串分隔。你可以发送的部分数量没有限制。

每个这样的部分都有一个名称、一组头和一些其他属性。

libcurl 提供了一套便利函数来构建这样的部分序列并将其发送到服务器，所有这些函数都以`curl_mime`为前缀。创建一个多部分表单，对于数据中的每个部分，你设置名称、数据和可能的其他元数据。一个基本的设置可能看起来像这样：

```sh
/* Create the form */
form = curl_mime_init(curl);

/* Fill in the file upload field */
field = curl_mime_addpart(form);
curl_mime_name(field, "sendfile");
curl_mime_filedata(field, "photo.jpg");
```

然后你将这个帖子传递给 libcurl，如下所示：

```sh
curl_easy_setopt(easy, CURLOPT_MIMEPOST, form);
```

(`curl_formadd`是之前用于构建多部分表单的 API，但我们不再推荐使用它)

## HTTP PUT

使用 libcurl 进行 PUT 操作时，它假定你通过读取回调传递数据给它，因为这是 libcurl 使用的典型“文件上传”模式，并且提供了该模式。你设置回调，请求 PUT（通过请求`CURLOPT_UPLOAD`），设置上传的大小，并设置目标 URL：

```sh
curl_easy_setopt(easy, CURLOPT_UPLOAD, 1L);
curl_easy_setopt(easy, CURLOPT_INFILESIZE_LARGE, (curl_off_t) size);
curl_easy_setopt(easy, CURLOPT_READFUNCTION, read_callback);
curl_easy_setopt(easy, CURLOPT_URL, "https://example.com/handle/put");
```

如果你在传输开始之前不知道上传的大小，并且你正在使用 HTTP 1.1，你可以通过 CURLOPT_HTTPHEADER 添加一个`Transfer-Encoding: chunked`头。对于 HTTP 1.0，你必须事先提供大小，而对于 HTTP 2 及以后版本，既不需要大小也不需要额外的头。

## 期望：标题

在使用 HTTP 1.1 进行 HTTP 上传时，在某些情况下，libcurl 会插入一个`Expect: 100-continue`头。此头为服务器提供了一种在早期拒绝传输的方式，从而避免客户端在服务器有机会拒绝之前发送大量数据。

如果使用`CURLOPT_UPLOAD`进行 HTTP 上传，或者如果要求进行 HTTP POST（其中主体大小未知或已知大于 1024 字节），libcurl 会添加一个头。

使用 libcurl 的客户端可以通过 CURLOPT_HTTPHEADER 选项显式禁用`Expect:`头部的使用。

在 HTTP/2 或 HTTP/3 中不使用此头部。

## 上传也会下载

HTTP 是一种即使你向其上传数据也能返回内容的协议 - 这取决于服务器来决定。响应数据甚至可能在上传完成之前就开始发送回客户端。
