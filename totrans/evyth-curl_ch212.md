# Drive with multi_socket

multi_socket is the extra spicy version of the regular multi interface and is designed for event-driven applications. Make sure you read the [Drive with multi interface](ch210.xhtml#transfers__drive__multi__md) section first.

multi_socket supports multiple parallel transfers—all done in the same single thread—and have been used to run several tens of thousands of transfers in a single application. It is usually the API that makes the most sense if you do a large number (>100 or so) of parallel transfers.

Event-driven in this case means that your application uses a system level library or setup that subscribes to a number of sockets and it lets your application know when one of those sockets are readable or writable and it tells you exactly which one.

This setup allows clients to scale up the number of simultaneous transfers much higher than with other systems, and still maintain good performance. The regular APIs otherwise waste far too much time scanning through lists of all the sockets.

## Pick one

There are numerous event based systems to select from out there, and libcurl is completely agnostic to which one you use. libevent, libev and libuv are three popular ones but you can also go directly to your operating system’s native solutions such as epoll, kqueue, /dev/poll, pollset or Event Completion.

## Many easy handles

Just like with the regular multi interface, you add easy handles to a multi handle with `curl_multi_add_handle()`. One easy handle for each transfer you want to perform.

You can add them at any time while the transfers are running and you can also similarly remove easy handles at any time using the `curl_multi_remove_handle` call. Typically though, you remove a handle only after its transfer is completed.

## multi_socket callbacks

As explained above, this event-based mechanism relies on the application to know which sockets that are used by libcurl and what activities libcurl waits for on those sockets: if it waits for the socket to become readable, writable or both.

The application also needs to tell libcurl when the timeout time has expired, as it is control of driving everything libcurl cannot do it itself. libcurl informs the application updated timeout values as soon as it needs to.

### socket_callback

libcurl informs the application about socket activity to wait for with a callback called [CURLMOPT_SOCKETFUNCTION](https://curl.se/libcurl/c/CURLMOPT_SOCKETFUNCTION.html). Your application needs to implement such a function:

[PRE0]

Using this, libcurl sets and removes sockets your application should monitor. Your application tells the underlying event-based system to wait for the sockets. This callback is called multiple times if there are multiple sockets to wait for, and it is called again when the status changes and perhaps you should switch from waiting for a writable socket to instead wait for it to become readable.

When one of the sockets that the application is monitoring on libcurl’s behalf registers that it becomes readable or writable, as requested, you tell libcurl about it by calling `curl_multi_socket_action()` and passing in the affected socket and an associated bitmask specifying which socket activity that was registered:

[PRE1]

### timer_callback

The application is in control and waits for socket activity. But even without socket activity there are things libcurl needs to do. Timeout things, calling the progress callback, starting over a retry or failing a transfer that takes too long, etc. To make that work, the application must also make sure to handle a single-shot timeout that libcurl sets.

libcurl sets the timeout with the timer_callback [CURLMOPT_TIMERFUNCTION](https://curl.se/libcurl/c/CURLMOPT_TIMERFUNCTION.html):

[PRE2]

There is only one timeout for the application to handle for the entire multi handle, no matter how many individual easy handles that have been added or transfers that are in progress. The timer callback gets updated with the current nearest-in-time period to wait. If libcurl gets called before the timeout expiry time because of socket activity, it may update the timeout value again before it expires.

When the event system of your choice eventually tells you that the timer has expired, you need to tell libcurl about it:

[PRE3]

…in many cases, this makes libcurl call the timer_callback again and set a new timeout for the next expiry period.

### How to start everything

When you have added one or more easy handles to the multi handle and set the socket and timer callbacks in the multi handle, you are ready to start the transfer.

To kick it all off, you tell libcurl it timed out (because all easy handles start out with a short timeout) which make libcurl call the callbacks to set things up and from then on you can just let your event system drive:

[PRE4]

### When is it done?

The ‘running_handles’ counter returned by `curl_multi_socket_action` holds the number of current transfers not completed. When that number reaches zero, we know there are no transfers going on.

Each time the ‘running_handles’ counter changes, `curl_multi_info_read()` returns info about the specific transfers that completed.