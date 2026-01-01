# 认证

libcurl 支持多种 HTTP 认证方案。

注意，这种认证方式与今天广泛使用的在 Web 上通过 HTTP POST 执行认证并在 cookies 中保持状态的方案不同。有关如何操作的详细信息，请参阅 libcurl 的 Cookies。

## 用户名和密码

libcurl 在没有给定用户名的情况下不会尝试任何 HTTP 认证。设置一个如下：

```sh
curl_easy_setopt(curl, CURLOPT_USERNAME, "joe");
```

当然，大多数认证还需要一个单独设置的密码：

```sh
curl_easy_setopt(curl, CURLOPT_PASSWORD, "secret");
```

这就是你需要做的。这使得 libcurl 为这次传输切换到其默认的认证方法：*HTTP Basic*。

## 认证要求

客户端本身不会决定发送一个认证请求。这是服务器要求的。当服务器有一个受保护的资源并需要认证时，它会以 401 HTTP 响应和一个`WWW-Authenticate:`头来响应。该头包括关于它为该资源接受哪些特定认证方法的详细信息。

## Basic

Basic 是默认的 HTTP 认证方法，正如其名所示，它确实是基本的。它接受用户名和密码，用冒号分隔它们，然后在将其整个放入请求中的`Authorization:` HTTP 头之前进行 base64 编码。

如果像上面示例中那样设置了用户名和密码，精确的输出头看起来像这样：

```sh
Authorization: Basic am9lOnNlY3JldA==
```

这种认证方法在 HTTP 上完全不安全，因为凭证以明文形式在网络中发送。

你可以明确告诉 libcurl 为特定的传输使用 Basic 方法，如下所示：

```sh
curl_easy_setopt(curl, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
```

## Digest

另一种 HTTP 认证方法被称为 Digest。与 Basic 相比，这种方法的一个优点是它不会在明文中将密码发送到线路上。然而，这是一个很少被浏览器提及的认证方法，因此也不是一个常用的方法。

你可以明确告诉 libcurl 为特定的传输使用 Digest 方法，如下所示（它仍然需要设置用户名和密码）：

```sh
curl_easy_setopt(curl, CURLOPT_HTTPAUTH, CURLAUTH_DIGEST);
```

## NTLM

另一种 HTTP 认证方法被称为 NTLM。

你可以明确告诉 libcurl 为特定的传输使用 NTLM 方法，如下所示（它仍然需要设置用户名和密码）：

```sh
curl_easy_setopt(curl, CURLOPT_HTTPAUTH, CURLAUTH_NTLM);
```

## Negotiate

另一种 HTTP 认证方法被称为 Negotiate。

你可以明确告诉 libcurl 为特定的传输使用 Negotiate 方法，如下所示（它仍然需要设置用户名和密码）：

```sh
curl_easy_setopt(curl, CURLOPT_HTTPAUTH, CURLAUTH_NEGOTIATE);
```

## Bearer

要在请求中传递 OAuth 2.0 Bearer 访问令牌，例如使用`CURLOPT_XOAUTH2_BEARER`：

```sh
CURL *curl = curl_easy_init();
if(curl) {
  curl_easy_setopt(curl, CURLOPT_URL, "pop3://example.com/");
  curl_easy_setopt(curl, CURLOPT_XOAUTH2_BEARER, "1ab9cb22ba269a7");
  ret = curl_easy_perform(curl);
  curl_easy_cleanup(curl);
}
```

## 尝试第一个

一些 HTTP 服务器允许使用多种认证方法中的一种，在某些情况下，你可能发现自己作为客户端，在事先无法选择单一特定方法，而对于另一个子集的情况，你的应用程序甚至不知道请求的 URL 是否需要认证。

libcurl 也涵盖了所有这些情况。

你可以要求 libcurl 使用多种方法，在这样做的时候，你暗示 curl 首先尝试不进行任何身份验证的请求，然后根据返回的 HTTP 响应，选择服务器和你的应用程序都允许的方法之一。如果有多种方法都可以工作，curl 会根据这些方法被认为有多安全来选择一个顺序，选择最安全的方法。

告诉 libcurl 通过按位或操作接受多种方法，就像这样：

```sh
curl_easy_setopt(curl, CURLOPT_HTTPAUTH,
                 CURLAUTH_BASIC | CURLAUTH_DIGEST);
```

如果你希望 libcurl 只允许使用一种特定的方法，但仍然希望它首先探测是否可以在不使用身份验证的情况下发送请求，你可以通过在掩码中添加`CURLAUTH_ONLY`来强制这种行为。

请求使用摘要，但除了摘要之外不使用其他任何方法，并且只有在证明确实有必要时：

```sh
curl_easy_setopt(curl, CURLOPT_HTTPAUTH,
                 CURLAUTH_DIGEST | CURLAUTH_ONLY);
```
