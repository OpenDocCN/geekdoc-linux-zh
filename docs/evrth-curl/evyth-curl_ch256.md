# alt-svc

替代服务，即 alt-svc，是一个 HTTP 头部，允许服务器通过使用 `Alt-Svc:` 响应头部告诉客户端，该服务器在另一个位置有一个或多个 *替代方案*。

服务器建议的 *替代方案* 可以包括在同一主机上运行的其他端口的服务器，也可以是在完全不同的主机名上运行的服务器，它还可以通过 *另一种协议* 提供服务。

## 启用

要让 libcurl 考虑服务器提供的任何替代方案，您必须首先在句柄中启用它。您可以通过将正确的掩码设置到 `CURLOPT_ALTSVC_CTRL` 选项来实现。掩码允许应用程序限制允许的 HTTP 版本，并确定磁盘上的缓存文件是否仅用于读取（不写入）。

启用 alt-svc 并允许其切换到 HTTP/1 或 HTTP/2：

```sh
curl_easy_setopt(curl, CURLOPT_ALTSVC_CTRL, CURLALTSVC_H1|CURLALTSVC_H2);
```

通过以下方式告诉 libcurl 使用特定的 alt-svc 缓存文件：

```sh
curl_easy_setopt(curl, CURLOPT_ALTSVC, "altsvc-cache.txt");
```

libcurl 将替代方案列表存储在基于内存的缓存中，但在启动时加载所有现有的替代服务条目从 alt-svc 文件，并在后续的 HTTP 请求时考虑这些条目。如果服务器响应新的或更新的 `Alt-Svc:` 头部，libcurl 将这些存储在退出时的缓存文件中（除非设置了 `CURLALTSVC_READONLYFILE` 位）。

## alt-svc 缓存

alt-svc 缓存类似于一个 cookie jar。它是一个基于文本的文件，每行存储一个替代方案，每个条目还有一个有效期，表示该特定替代方案的有效时长。

## 仅 HTTPS

`Alt-Svc:` 只有在通过 HTTPS 连接时才被信任并解析来自服务器的信息。

## HTTP/3

截至 2022 年 3 月，使用 `Alt-Svc:` 头部仍然是启动客户端和服务器使用 HTTP/3 的唯一定义方式。然后服务器通过 HTTP/1 或 HTTP/2 向客户端暗示它也支持 HTTP/3，如果 alt-svc 缓存允许，curl 可以在后续请求中使用 HTTP/3 连接到它。
