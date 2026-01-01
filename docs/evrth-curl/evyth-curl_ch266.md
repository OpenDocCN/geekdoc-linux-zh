# 获取 URL 部分

`CURLU`句柄存储 URL 的各个部分，应用程序可以在任何时间从句柄中单独提取这些部分。如果它们被设置。

`curl_url_get()`的第二个参数指定了您想要提取的部分。它们都被提取为空终止的`char *`数据，因此您需要传递一个指向此类变量的指针。

```sh
char *host;
rc = curl_url_get(h, CURLUPART_HOST, &host, 0);

char *scheme;
rc = curl_url_get(h, CURLUPART_SCHEME, &scheme, 0);

char *user;
rc = curl_url_get(h, CURLUPART_USER, &user, 0);

char *password;
rc = curl_url_get(h, CURLUPART_PASSWORD, &password, 0);

char *port;
rc = curl_url_get(h, CURLUPART_PORT, &port, 0);

char *path;
rc = curl_url_get(h, CURLUPART_PATH, &path, 0);

char *query;
rc = curl_url_get(h, CURLUPART_QUERY, &query, 0);

char *fragment;
rc = curl_url_get(h, CURLUPART_FRAGMENT, &fragment, 0);

char *zoneid;
rc = curl_url_get(h, CURLUPART_ZONEID, &zoneid, 0);
```

记得使用`curl_free`释放返回的字符串，当你完成使用时！

提取的部分除非用户使用`CURLU_URLDECODE`标志请求，否则不会进行 URL 解码。

## URL 部分

不同的部分是根据它们在 URL 中的角色命名的。想象一个看起来像这样的 URL：

```sh
http://joe:7Hbz@example.com:8080/images?id=5445#footer
```

当这个 URL 被 curl 解析时，它会以这种方式存储不同的组件：

| 文本 | 部分 |
| --- | --- |
| `http` | `CURLUPART_SCHEME` |
| `joe` | `CURLUPART_USER` |
| `7Hbz` | `CURLUPART_PASSWORD` |
| `example.com` | `CURLUPART_HOST` |
| `8080` | `CURLUPART_PORT` |
| `/images` | `CURLUPART_PATH` |
| `id=5445` | `CURLUPART_QUERY` |
| `footer` | `CURLUPART_FRAGMENT` |

## 区域 ID

可能会稍微突出一点的是区域 ID。它是一个额外的限定符，可以用于 IPv6 数值地址，并且仅适用于此类地址。它的使用方式如下，其中设置为`eth0`：

```sh
http://[2a04:4e42:e00::347%25eth0]/
```

对于这个 URL，curl 提取：

| 文本 | 部分 |
| --- | --- |
| `http` | `CURLUPART_SCHEME` |
| `2a04:4e42:e00::347` | `CURLUPART_HOST` |
| `eth0` | `CURLUPART_ZONEID` |
| `/` | `CURLUPART_PATH` |

请求任何其他组件都会返回非零值，因为它们缺失。
