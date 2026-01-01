# 使用 multi_socket 驱动

multi_socket 是常规多接口的增强版本，专为事件驱动应用程序设计。请确保您首先阅读使用多接口驱动部分。

multi_socket 支持多个并行传输——所有这些都在同一个单线程中完成——并且已被用于在单个应用程序中运行数万个传输。如果您进行大量并行传输（>100 个左右），通常这是最有意义的 API。

在这种情况下，事件驱动意味着您的应用程序使用一个系统级库或设置，它订阅了多个套接字，并在其中一个套接字可读或可写时通知您的应用程序，并确切地告诉您是哪一个。

这种设置允许客户端将同时传输的数量扩展得比其他系统高得多，同时仍然保持良好的性能。常规 API 否则会浪费太多时间扫描所有套接字列表。

## 选择一个

在外部有众多基于事件的选择，libcurl 对您使用哪一个完全无偏见。libevent、libev 和 libuv 是三个流行的选择，但您也可以直接使用操作系统的原生解决方案，如 epoll、kqueue、/dev/poll、pollset 或 Event Completion。

## 许多简单句柄

就像使用常规的多接口一样，您可以使用`curl_multi_add_handle()`将简单句柄添加到多句柄中。每个句柄对应您想要执行的一个传输。

您可以在传输运行期间随时添加它们，您也可以使用`curl_multi_remove_handle`调用在任意时间移除简单句柄。不过，通常您只在传输完成后才移除句柄。

## 多套接字回调

如上所述，这种基于事件的机制依赖于应用程序知道 libcurl 使用的套接字以及 libcurl 在这些套接字上等待的活动：如果它等待套接字变为可读、可写或两者。

应用程序还需要在超时时间到期时通知 libcurl，因为它控制着 libcurl 无法自行完成的一切。libcurl 在需要时立即通知应用程序更新的超时值。

### 套接字回调

libcurl 通过一个名为[CURLMOPT_SOCKETFUNCTION](https://curl.se/libcurl/c/CURLMOPT_SOCKETFUNCTION.html)的回调函数通知应用程序等待的套接字活动。您的应用程序需要实现这样一个函数：

```sh
int socket_callback(CURL *easy,      /* easy handle */
                    curl_socket_t s, /* socket */
                    int what,        /* what to wait for */
                    void *userp,     /* private callback pointer */
                    void *socketp)   /* private socket pointer */
{
   /* told about the socket 's' */
}

/* set the callback in the multi handle */
curl_multi_setopt(multi_handle, CURLMOPT_SOCKETFUNCTION, socket_callback);
```

使用此方法，libcurl 设置和删除应用程序应监视的套接字。您的应用程序告诉底层基于事件的系统等待套接字。如果有多个套接字要等待，此回调会被多次调用，当状态改变并且您可能需要从等待可写套接字切换到等待它变为可读时，它会被再次调用。

当应用程序代表监控的 libcurl 上的一个套接字注册它变得可读或可写时，你通过调用 `curl_multi_socket_action()` 并传递受影响的套接字以及一个相关的位掩码来告诉 libcurl，该位掩码指定了已注册的套接字活动：

```sh
int running_handles;
ret = curl_multi_socket_action(multi_handle,
                               sockfd, /* the socket with activity */
                               ev_bitmask, /* the specific activity */
                               &running_handles);
```

### timer_callback

应用程序处于控制状态，等待套接字活动。但即使没有套接字活动，libcurl 也需要做一些事情。比如处理超时，调用进度回调，重新尝试或失败耗时过长的传输等。为了使这些工作正常进行，应用程序还必须确保处理 libcurl 设置的单一超时。

libcurl 使用 timer_callback [CURLMOPT_TIMERFUNCTION](https://curl.se/libcurl/c/CURLMOPT_TIMERFUNCTION.html) 设置超时：

```sh
int timer_callback(multi_handle,   /* multi handle */
                   timeout_ms,     /* milliseconds to wait */
                   userp)          /* private callback pointer */
{
  /* the new time-out value to wait for is in 'timeout_ms' */
}

/* set the callback in the multi handle */
curl_multi_setopt(multi_handle, CURLMOPT_TIMERFUNCTION, timer_callback);
```

对于整个多句柄，应用程序只需处理一个超时，无论添加了多少个单独的简单句柄或正在进行多少传输。定时器回调会更新为等待的当前最近时间间隔。如果由于套接字活动在超时到期之前调用 libcurl，它可能会在超时到期之前再次更新超时值。

当你选择的的事件系统最终告诉你定时器已超时时，你需要通知 libcurl：

```sh
curl_multi_socket_action(multi, CURL_SOCKET_TIMEOUT, 0, &running);
```

…在许多情况下，这会使 libcurl 再次调用 timer_callback 并设置下一个到期周期的超时。

### 如何开始一切

当你在多句柄中添加了一个或多个简单句柄，并设置了句柄和定时器回调函数后，你就可以开始传输了。

要启动整个过程，你告诉 libcurl 它已超时（因为所有简单句柄最初都有一个短的超时时间），这将使 libcurl 调用回调来设置事情，从那时起你就可以让你的事件系统驱动：

```sh
/* all easy handles and callbacks are setup */

curl_multi_socket_action(multi, CURL_SOCKET_TIMEOUT, 0, &running);

/* now the callbacks should have been called and we have sockets to wait
   for and possibly a timeout, too. Make the event system do its magic */

event_base_dispatch(event_base); /* libevent2 has this API */

/* at this point we have exited the event loop */
```

### 它何时完成？

`curl_multi_socket_action` 返回的 ‘running_handles’ 计数器保持当前未完成的传输数量。当这个数字达到零时，我们知道没有传输正在进行。

每当 ‘running_handles’ 计数器发生变化时，`curl_multi_info_read()` 返回有关已完成的特定传输的信息。
