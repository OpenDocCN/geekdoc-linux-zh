# 在句柄之间共享数据

有时应用程序需要在传输之间共享数据。添加到同一多句柄的所有简单句柄会自动在同一个多句柄中的句柄之间完成大量的共享，但有时这并不是你想要的。

## 多句柄

所有添加到同一多句柄的简单句柄自动共享 连接缓存 和 dns 缓存。

## 简单句柄之间的共享

libcurl 有一个通用的“共享接口”，其中应用程序创建一个“共享对象”，然后可以由任意数量的简单句柄共享数据。数据随后从共享对象中存储和读取，而不是保留在共享数据的句柄中。

```sh
CURLSH *share = curl_share_init();
```

共享对象可以设置为共享所有或任何 cookie、连接缓存、dns 缓存和 SSL 会话 ID 缓存。

例如，设置共享以保存 cookie 和 dns 缓存：

```sh
curl_share_setopt(share, CURLSHOPT_SHARE, CURL_LOCK_DATA_COOKIE);
curl_share_setopt(share, CURLSHOPT_SHARE, CURL_LOCK_DATA_DNS);
```

然后设置相应的传输以使用此共享对象：

```sh
curl_easy_setopt(curl, CURLOPT_SHARE, share);
```

使用此 `curl` 句柄完成的传输会将其 cookie 和 dns 信息存储在 `share` 句柄中。你可以设置多个简单句柄以共享相同的共享对象。

## 要共享的内容

`CURL_LOCK_DATA_COOKIE` - 设置此位以共享 cookie jar。请注意，每个简单句柄仍然需要正确启动其 cookie “引擎”才能开始使用 cookie。

`CURL_LOCK_DATA_DNS` - DNS 缓存是 libcurl 存储解析的主机名的地址的地方，以便在后续查找中更快。

`CURL_LOCK_DATA_SSL_SESSION` - SSL 会话 ID 缓存是 libcurl 存储用于 SSL 连接的恢复信息的地方，以便能够更快地恢复之前的连接。

`CURL_LOCK_DATA_CONNECT` - 当设置时，此句柄使用共享连接缓存，因此更有可能找到现有的连接以重新使用等，这可能在以串行方式对同一主机进行多次传输时提高性能。

## 锁定

如果你想在多线程环境中让传输共享共享对象。也许你有一个具有许多核心的 CPU，你希望每个核心运行其自己的线程并传输数据，但你仍然希望不同的传输共享数据。那么你需要设置互斥回调。

如果你没有使用线程并且 *知道* 你以串行方式逐个访问共享对象，则不需要设置任何锁。但如果同时有多个传输访问共享对象，则需要设置互斥回调以防止数据损坏甚至崩溃。

由于 libcurl 本身不知道如何锁定事物或甚至不知道你使用的是哪种线程模型，你必须确保只允许一次访问的互斥锁。一个用于 pthreads 应用程序的锁回调可能如下所示：

```sh
static void lock_cb(CURL *handle, curl_lock_data data,
                    curl_lock_access access, void *userptr)
{
  pthread_mutex_lock(&lock[data]); /* uses a global lock array */
}
curl_share_setopt(share, CURLSHOPT_LOCKFUNC, lock_cb);
```

相应的解锁回调可能如下所示：

```sh
static void unlock_cb(CURL *handle, curl_lock_data data,
                      void *userptr)
{
  pthread_mutex_unlock(&lock[data]); /* uses a global lock array */
}
curl_share_setopt(share, CURLSHOPT_UNLOCKFUNC, unlock_cb);
```

## 取消共享

在传输过程中，传输使用共享对象，并将该对象被指定共享的内容与其他共享相同对象的句柄共享。

在后续传输中，可以将`CURLOPT_SHARE`设置为 NULL 以防止传输继续共享。在这种情况下，句柄可能以空缓存开始下一次传输，这些缓存是之前共享的数据。

在两次传输之间，共享对象也可以更新以共享不同的属性集，这样共享该对象的句柄在下一次共享时将共享不同的数据集。当取消共享 DNS 数据时，可以使用 curl_share_setopt()的`CURLSHOPT_UNSHARE`选项从共享对象中移除要共享的项目：

```sh
curl_share_setopt(share, CURLSHOPT_UNSHARE, CURL_LOCK_DATA_DNS);
```
