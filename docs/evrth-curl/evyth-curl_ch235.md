# 名称解析

大多数 libcurl 可以执行的数据传输都涉及一个首先需要解析为互联网地址的名称。这就是名称解析。在 URL 中直接使用数值 IP 地址通常可以避免名称解析阶段，但在许多情况下，手动将名称替换为 IP 地址并不容易。

libcurl 尽力重用现有连接而不是创建一个新的连接。检查现有连接以使用的函数仅基于名称，并在尝试任何名称解析之前执行。这就是重用如此之快的一个原因。使用重用连接的传输不会再次解析主机名。

如果无法重用连接，libcurl 将将主机名解析为它解析到的地址集。通常这意味着请求 IPv4 和 IPv6 地址，并且可能有一整套这些地址返回给 libcurl。然后尝试这一组地址，直到找到一个可以工作的，或者返回失败。

应用程序可以通过将 `CURLOPT_IPRESOLVE` 设置为首选值来强制 libcurl 只使用 IPv4 或 IPv6 解析的地址。例如，请求只使用 IPv6 地址：

```sh
curl_easy_setopt(easy, CURLOPT_IPRESOLVE, CURL_IPRESOLVE_V6);
```

## 名称解析后端

libcurl 可以构建为以三种不同方式之一进行名称解析，具体取决于使用的后端方式，它将获得略有不同的功能集和有时修改后的行为。

1.  默认后端是在新的辅助线程中调用正常的 libc 解析器函数，这样它仍然可以在需要时执行细粒度超时，并且没有阻塞调用。

1.  在较旧的系统上，libcurl 使用标准的同步名称解析函数。不幸的是，它们在其操作期间会在多处理块中执行所有传输，这使得优雅地超时变得非常困难。

1.  还支持使用 c-ares 第三方库进行解析，该库支持不使用线程的异步名称解析。这对于大量并行传输的扩展性更好，但它并不总是与原生名称解析功能完全兼容。

### DNS over HTTPS

不论 libcurl 构建时使用的是哪种解析器后端，从 7.62.0 版本开始，它还提供了一个让用户请求特定 DoH（DNS over HTTPS）服务器获取名称地址的方法。这避免了使用正常的、原生的解析器方法和服务器，而是请求一个专门的独立服务器。

使用 `CURLOPT_DOH_URL` 选项指定一个完整的 URL 作为 DoH 服务器，例如：

```sh
curl_easy_setopt(easy, CURLOPT_DOH_URL, "https://example.com/doh");
```

传递给此选项的 URL *必须* 使用 https://，并且通常建议您启用 HTTP/2 支持，以便 libcurl 可以在连接到 DoH 服务器的连接上多路复用执行多个 DoH 请求。

## 缓存

当一个名称被解析后，结果将存储在 libcurl 的内存缓存中，以便后续对该名称的解析可以即时完成，只要该名称保留在 DNS 缓存中。默认情况下，每个条目在缓存中保留 60 秒，但可以使用`CURLOPT_DNS_CACHE_TIMEOUT`更改此值。

当使用`curl_easy_perform`时，DNS 缓存保留在 easy 句柄中，或者当使用多接口时，保留在 multi 句柄中。还可以使用共享接口在多个 easy 句柄之间共享 DNS 缓存。

## 主机的自定义地址

有时提供虚假的自定义地址以代替真实的主机名很有用，这样 libcurl 就会连接到不同的地址，而不是实际名称解析所建议的地址。

通过使用`CURLOPT_RESOLVE`选项，应用程序可以预先填充 libcurl 的 DNS 缓存，为特定的主机名和端口号指定一个自定义地址。

要使 libcurl 在请求 example.com 的 443 端口时连接到 127.0.0.1，应用程序可以执行以下操作：

```sh
struct curl_slist *dns;
dns = curl_slist_append(NULL, "example.com:443:127.0.0.1");
curl_easy_setopt(curl, CURLOPT_RESOLVE, dns);
```

由于这会将虚假地址放入 DNS 缓存中，因此即使在跟随重定向等情况下也能正常工作。

## 名称服务器选项

对于构建为使用 c-ares 的 libcurl，有一些选项可供选择，这些选项提供了对要使用的 DNS 服务器及其使用方式的精细控制。这仅限于纯 c-ares 构建，因为这些功能在标准系统调用用于名称解析时不可用。

+   使用`CURLOPT_DNS_SERVERS`，应用程序可以选择使用一组专用的 DNS 服务器。

+   使用`CURLOPT_DNS_INTERFACE`，它可以告诉 libcurl 使用哪个网络接口来代替默认的 DNS 接口。

+   使用`CURLOPT_DNS_LOCAL_IP4`和`CURLOPT_DNS_LOCAL_IP6`，应用程序可以指定要将 DNS 解析绑定到哪个特定的网络地址。

## 无全局 DNS 缓存

之前名为`CURLOPT_DNS_USE_GLOBAL_CACHE`的选项曾指示 curl 使用全局 DNS 缓存。自 7.65.0 版本以来，此功能已被移除，因此尽管此选项仍然存在，但它不再执行任何操作。
