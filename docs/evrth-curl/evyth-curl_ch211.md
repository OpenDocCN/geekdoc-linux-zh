# 使用多接口驱动

“多”这个名字是表示多个，比如多个并行传输，所有这些都在同一个单独的线程中完成。多 API 是非阻塞的，因此也可以用于单次传输。

传输仍然设置在如上所述的“简单”的`CURL *`句柄中，但使用多接口时，你还需要创建一个多`CURLM *`句柄，并使用它来驱动所有单个传输。多句柄可以“持有”一个或多个简单句柄：

```sh
CURLM *multi_handle = curl_multi_init();
```

多句柄也可以设置某些选项，这通过`curl_multi_setopt()`完成，但在最简单的情况下，你可能没有什么可以设置的。

要驱动多接口传输，你首先需要将所有应该传输的单个简单句柄添加到多句柄中。你可以在任何时候将它们添加到多句柄中，也可以随时将它们移除。从多句柄中移除简单句柄会取消关联，并且那个特定的传输会立即停止。

将简单句柄添加到多句柄中很简单：

```sh
curl_multi_add_handle( multi_handle, easy_handle );
```

移除一个也很简单：

```sh
curl_multi_remove_handle( multi_handle, easy_handle );
```

添加了代表你想要执行传输的简单句柄后，你编写传输循环。使用多接口，你进行循环，这样你可以要求 libcurl 提供一组文件描述符和一个超时值，然后你自己进行`select()`调用，或者你可以使用稍微简化一点的版本，它为我们做这件事，使用`curl_multi_wait`。最简单的循环可能看起来像这样：（注意，实际应用会检查返回代码）

```sh
int transfers_running;
do {
   curl_multi_wait ( multi_handle, NULL, 0, 1000, NULL);
   curl_multi_perform ( multi_handle, &transfers_running );
} while (transfers_running);
```

`curl_multi_wait`的第四个参数，在上面的例子中设置为 1000，是一个以毫秒为单位的超时值。这是函数在返回之前等待任何活动可能的最长时间。你不想在再次调用`curl_multi_perform`之前锁定太长时间，因为存在超时、进度回调等，这样做可能会失去精度。

要自行执行 select()，我们像这样从 libcurl 中提取文件描述符和超时值（注意，实际应用会检查返回代码）：

```sh
int transfers_running;
do {
  fd_set fdread;
  fd_set fdwrite;
  fd_set fdexcep;
  int maxfd = -1;
  long timeout;

  /* extract timeout value */
  curl_multi_timeout(multi_handle, &timeout);
  if (timeout < 0)
    timeout = 1000;

  /* convert to struct usable by select */
  timeout.tv_sec = timeout / 1000;
  timeout.tv_usec = (timeout % 1000) * 1000;

  FD_ZERO(&fdread);
  FD_ZERO(&fdwrite);
  FD_ZERO(&fdexcep);

  /* get file descriptors from the transfers */
  mc = curl_multi_fdset(multi_handle, &fdread, &fdwrite,
                        &fdexcep, &maxfd);

  if (maxfd == -1) {
    SHORT_SLEEP;
  }
  else
   select(maxfd+1, &fdread, &fdwrite, &fdexcep, &timeout);

  /* timeout or readable/writable sockets */
  curl_multi_perform(multi_handle, &transfers_running);
} while ( transfers_running );
```

这两个循环都让你可以使用一个或多个自己的文件描述符来等待，比如如果你从自己的套接字或管道或类似的东西中读取。

再次强调，你可以在循环的任何时刻添加和移除简单句柄到多句柄。在传输过程中移除句柄会中止该传输。

## 单次传输何时完成？

如上例所示，程序可以通过查看`transfers_running`变量减少来检测单个传输何时完成。

它还可以调用`curl_multi_info_read()`，如果传输结束，它会返回一个指向结构体（一个“消息”）的指针，然后你可以使用该结构体找出该传输的结果。

当你进行多个并行传输时，当然可能会有多个传输在同一个 `curl_multi_perform` 调用中完成，然后你可能需要多次调用 `curl_multi_info_read` 来获取每个已完成的传输的信息。
