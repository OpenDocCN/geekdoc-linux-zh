# 请求

HTTP 请求是 curl 向服务器发送以告知服务器要执行的操作。当它想要获取数据或发送数据时，所有涉及 HTTP 的传输都以 HTTP 请求开始。

一个 HTTP 请求包含一个方法、一个路径、HTTP 版本和一组请求头。使用 libcurl 的应用程序可以调整所有这些字段。

## 请求方法

每个 HTTP 请求都包含一个“方法”，有时也称为“动词”。它通常是 GET、HEAD、POST 或 PUT 等内容，但也有更专业的如 DELETE、PATCH 和 OPTIONS。

通常，当您使用 libcurl 设置和执行传输时，特定的请求方法由您使用的选项隐含。如果您只是请求一个 URL，这意味着方法是 `GET`，而如果您设置例如 `CURLOPT_POSTFIELDS`，这将使 libcurl 使用 `POST` 方法。如果您将 `CURLOPT_UPLOAD` 设置为 true，libcurl 在其 HTTP 请求中发送一个 `PUT` 方法，依此类推。请求 `CURLOPT_NOBODY` 将使 libcurl 使用 `HEAD`。

然而，有时那些默认的 HTTP 方法不足以满足需求，或者根本不是您想要的传输方法。然后您可以指示 libcurl 使用您喜欢的特定方法，通过 `CURLOPT_CUSTOMREQUEST`。例如，您想向您选择的 URL 发送一个 `DELETE` 方法：

```sh
curl_easy_setopt(curl, CURLOPT_CUSTOMREQUEST, "DELETE");
curl_easy_setopt(curl, CURLOPT_URL, "https://example.com/file.txt");
```

CURLOPT_CUSTOMREQUEST 设置应仅是作为 HTTP 请求行中方法的单个关键字。如果您想更改或添加额外的 HTTP 请求头，请参阅以下部分。

## 自定义 HTTP 请求头

当 libcurl 在执行您请求的数据传输时发出 HTTP 请求，它会使用一组适合完成分配给它的任务的 HTTP 头信息发送请求。

如果只提供了 `http://localhost/file1.txt` 这个 URL，libcurl 将向服务器发送以下请求：

```sh
GET /file1.txt HTTP/1.1
Host: localhost
Accept: */*
```

如果您指示应用程序也将 `CURLOPT_POSTFIELDS` 设置为字符串 “foobar”（6 个字母，引号仅用于视觉分隔符），它将发送以下头信息：

```sh
POST /file1.txt HTTP/1.1
Host: localhost
Accept: */*
Content-Length: 6
Content-Type: application/x-www-form-urlencoded
```

如果您对 libcurl 发送的默认头信息集不满意，应用程序有权在 HTTP 请求中添加、更改或删除头信息。

### 添加一个头

要添加一个在请求中不会出现的头，使用 `CURLOPT_HTTPHEADER` 添加它。假设您想要一个名为 `Name:` 的头，它包含 `Mr. Smith`：

```sh
struct curl_slist *list = NULL;
list = curl_slist_append(list, "Name: Mr Smith");
curl_easy_setopt(curl, CURLOPT_HTTPHEADER, list);
curl_easy_perform(curl);
curl_slist_free_all(list); /* free the list again */
```

### 更改一个头

如果默认的标题之一不符合您的期望，您可以修改它们。例如，如果您认为默认的 `Host:` 标题是错误的（即使它是从您提供的 libcurl URL 中派生出来的），您可以告诉 libcurl 使用您自己的：

```sh
struct curl_slist *list = NULL;
list = curl_slist_append(list, "Host: Alternative");
curl_easy_setopt(curl, CURLOPT_HTTPHEADER, list);
curl_easy_perform(curl);
curl_slist_free_all(list); /* free the list again */
```

### 移除一个头

当您认为 libcurl 在请求中使用了一个您认为不应该使用的头时，您可以轻松地告诉它从请求中移除它。例如，如果您想移除 `Accept:` 头。只需提供头名称，冒号右边不提供任何内容：

```sh
struct curl_slist *list = NULL;
list = curl_slist_append(list, "Accept:");
curl_easy_setopt(curl, CURLOPT_HTTPHEADER, list);
curl_easy_perform(curl);
curl_slist_free_all(list); /* free the list again */
```

### 提供一个无内容的头

如您在上文各节中可能已经注意到的，如果您尝试在冒号右侧添加没有内容的标题，它会被视为删除指令，从而完全阻止该标题被发送。如果您确实*真正地*想要发送一个右侧内容为零的标题，您需要使用一个特殊的标记。您必须使用分号而不是正确的冒号来提供标题。例如`Header;`。如果您想要向发出的 HTTP 请求添加一个只有`Moo:`而没有冒号后跟内容的标题，您可以这样写：

```sh
struct curl_slist *list = NULL;
list = curl_slist_append(list, "Moo;");
curl_easy_setopt(curl, CURLOPT_HTTPHEADER, list);
curl_easy_perform(curl);
curl_slist_free_all(list); /* free the list again */
```

## 引用者

`Referer:`标题（是的，它拼写错误）是一个标准的 HTTP 标题，它告诉服务器用户代理是从哪个 URL 被引导到它现在请求的 URL 的。它是一个正常的标题，因此您可以使用上面显示的`CURLOPT_HEADER`方法自己设置它，或者您可以使用称为`CURLOPT_REFERER`的快捷方式。就像这样：

```sh
curl_easy_setopt(curl, CURLOPT_REFERER, "https://example.com/fromhere/");
curl_easy_perform(curl);
```

### 自动引用者

当 libcurl 被要求使用`CURLOPT_FOLLOWLOCATION`选项自行跟随重定向，并且您仍然希望将`Referer:`标题设置为它进行重定向的正确的前一个 URL，您可以要求 libcurl 自行设置该值：

```sh
curl_easy_setopt(curl, CURLOPT_FOLLOWLOCATION, 1L);
curl_easy_setopt(curl, CURLOPT_AUTOREFERER, 1L);
curl_easy_setopt(curl, CURLOPT_URL, "https://example.com/redirected.cgi");
curl_easy_perform(curl);
```
