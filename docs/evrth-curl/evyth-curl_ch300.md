# 结构体

本节记录了内部结构体。由于它们确实是内部的，我们偶尔会更改它们，这可能会使本节在某些时候略显过时。

## Curl_easy

`Curl_easy` 结构体是在外部 API 中以不透明的 `CURL *` 形式返回的。这个指针通常在 API 文档和示例中被称为简单句柄。

与实际连接相关的信息和状态在 `connectdata` 结构体中。当即将进行传输时，libcurl 要么创建一个新的连接，要么重用现有的一个。当前由这个句柄使用的当前连接数据由 `Curl_easy->conn` 指出。

与这个特定单次传输相关的数据和信息被放入 `SingleRequest` 子结构体中。

当 `Curl_easy` 结构体被添加到多句柄中，为了执行任何传输，它必须这样做，`->multi` 成员指向它所属的 `Curl_multi` 结构体。`->prev` 和 `->next` 成员随后被多代码用来保持添加到同一多句柄的 `Curl_easy` 结构体的链表。libcurl 始终使用多句柄，因此在传输进行时 `->multi` 指向一个 `Curl_multi`。

`->mstate` 是这个特定 `Curl_easy` 的多状态。当调用 `multi_runsingle()` 时，它根据当前状态对此句柄进行操作。mstate 还告诉在调用 `curl_multi_fdset()` 等时，为特定的 `Curl_easy` 返回哪些套接字。

libcurl 源代码通常使用 `data` 这个名字来命名指向 `Curl_easy` 结构体的局部变量。

在进行多路复用的 HTTP/2 传输时，每个 `Curl_easy` 都与一个单独的流相关联，共享相同的连接数据结构。多路复用使得保持与正确事物关联变得更加重要。

## connectdata

在 libcurl 中，一个普遍的想法是在连接使用后将其保留在连接缓存中，以防再次使用，然后重用现有的连接而不是创建一个新的，因为这可以显著提高性能。

每个 `connectdata` 结构体标识了对服务器的单个物理连接。如果连接不能保持活动状态，则在使用后关闭连接，然后可以从缓存中删除此结构体并释放它。

因此，同一个 `Curl_easy` 可以被多次使用，每次可以选择另一个 `connectdata` 结构体来用于连接。请记住这一点，因为这很重要，需要考虑选项或选择是基于连接还是 `Curl_easy`。

作为一种特殊的复杂性，libcurl 支持的一些协议需要一种特殊的断开连接程序，这不仅仅是关闭套接字。这可能涉及到在这样做之前向服务器发送一个或多个命令。由于连接在使用后被保留在连接缓存中，因此在关闭特定连接的时候，原始的`Curl_easy`可能已经不再存在。为此，libcurl 在`Curl_multi`结构中保留了一个特殊的虚拟`closure_handle` `Curl_easy`，以便在需要时使用。

FTP 使用两个 TCP 连接进行典型传输，但它将这两个连接都保留在这个单个结构中，因此可以被认为是大多数内部关注的一个单一连接。

libcurl 的源代码通常使用`conn`这个名字来表示指向 connectdata 的局部变量。

## Curl_multi

在内部，easy 接口被实现为多接口函数的包装器。这使得一切都是多接口。

`Curl_multi`是多句柄结构，在外部 API 中以不透明的`CURLM *`暴露。

这个结构包含了一个`Curl_easy`结构体的列表，这些结构体是通过`curl_multi_add_handle()`[13]添加到这个句柄的。列表的起始点是`->easyp`，而`->num_easy`是添加的`Curl_easy`的计数器。

`->msglist`是一个消息链表，当调用`curl_multi_info_read()`[14]时用来发送回消息。基本上，当单个`Curl_easy`的传输完成时，就会向该列表添加一个节点。

`->hostcache`指向名称缓存。这是一个用于查找名称到 IP 的散列表。节点在那里有有限的生存期，这个缓存旨在减少在短时间内需要相同名称的时间。

`->timetree`指向一个`Curl_easy`的树，按剩余时间直到应该检查排序——通常是某种超时。每个`Curl_easy`在树中有一个节点。

`->sockhash`是一个散列表，允许快速查找`Curl_easy`使用的套接字描述符。这对于`multi_socket` API 是必要的。

`->conn_cache`指向连接缓存。它跟踪所有在使用后被保留的连接。缓存有一个最大大小。

`->closure_handle`在`connectdata`部分有描述。

libcurl 的源代码通常使用`multi`这个名字来表示指向`Curl_multi`结构的变量。

## Curl_handler

libcurl 支持的每个独特协议都需要提供一个至少包含一个`Curl_handler`结构的实现。它定义了协议的名称以及主代码应该调用哪些函数来处理特定协议的问题。通常，有一个名为`[protocol].c`的源文件，其中声明了一个`struct Curl_handler Curl_handler_[protocol]`结构。在`url.c`中，有一个主数组，其中包含所有指向单个数组的`Curl_handler`结构体，当给 libcurl 一个 URL 进行处理时，会扫描这个数组。

具体的函数指针原型可以在`lib/urldata.h`中找到。

+   `->scheme` 是 URL 方案名称，通常用大写字母拼写。例如 HTTP 或 FTP 等。协议的 SSL 版本需要自己的 `Curl_handler` 设置，因此 HTTPS 与 HTTP 分开。

+   `->setup_connection` 被调用以允许协议代码分配特定于协议的数据，这些数据随后与整个传输过程中的 `Curl_easy` 关联。在传输结束时再次释放。它在选择/创建传输的 `connectdata` 之前被调用。大多数协议在这里分配其私有的 `struct [PROTOCOL]` 并将其分配给 `Curl_easy->req.p.[protocol]`。

+   `->connect_it` 允许协议在 TCP 连接完成后执行一些特定操作，这些操作仍可被视为连接阶段的一部分。一些协议在此函数中更改 `connectdata->recv[]` 和 `connectdata->send[]` 函数指针。

+   `->connecting` 是一个函数，只要协议认为它仍然处于连接阶段，就会不断被调用。

+   `->do_it` 是调用以发出传输请求的函数。我们内部称之为 DO 操作。如果 DO 不足以完成整个 DO 序列，则通常还提供 `->doing`。每个需要执行多个命令或类似操作的协议都需要实现自己的状态机（参见 SCP、SFTP、FTP）。一些协议（只有 FTP，并且仅由于历史原因）有一个单独的 DO 状态部分，称为 `DO_MORE`。

+   `->doing` 在发出传输请求命令时不断被调用

+   `->done` 在传输完成并标记为完成时被调用。即在主要数据传输之后。

+   `->do_more` 在 `DO_MORE` 状态期间被调用。FTP 协议在设置第二个连接时使用此状态。

+   `->proto_getsock`、`->doing_getsock`、`->domore_getsock`、`->perform_getsock` 函数返回套接字信息。在特定多状态期间等待哪个套接字进行哪些 I/O 操作。

+   `->disconnect` 在关闭 TCP 连接之前立即被调用。

+   `->readwrite` 在传输期间被调用，允许协议执行额外的读取/写入操作

+   `->attach` 将传输附加到连接上。

+   `->defport` 是此协议使用的默认报告 TCP 或 UDP 端口

+   `->protocol` 是 `CURLPROTO_*` 集中的一个或多个位。SSL 版本具有基本协议集和 SSL 变体。例如 `HTTP|HTTPS`。

+   `->flags` 是一个掩码，包含有关协议的附加信息，这使得通用引擎以不同的方式处理它：

    +   `PROTOPT_SSL` - 使其连接并协商 SSL

    +   `PROTOPT_DUAL` - 此协议使用两个连接

    +   `PROTOPT_CLOSEACTION` - 此协议在关闭连接之前执行操作。此标志不再由代码使用，但仍被许多协议处理程序设置。

    +   `PROTOPT_DIRLOCK` - 方向锁。SSH 协议设置这个位以限制主引擎关注的套接字动作的方向。

    +   `PROTOPT_NONETWORK` - 不使用网络的协议（读取 `file:`）

    +   `PROTOPT_NEEDSPWD` - 这个协议需要一个密码，如果没有提供，则使用默认密码

    +   `PROTOPT_NOURLQUERY` - 这个协议无法处理 URL 上的查询部分（?foo=bar）

## conncache

这是一个带有连接以供以后重用的哈希表。每个 `Curl_easy` 都有一个指向其连接缓存的指针。每个多处理句柄默认情况下都会设置一个连接缓存，所有添加的 `Curl_easy`s 都会共享这个缓存。

## Curl_share

libcurl 共享 API 分配一个 `Curl_share` 结构，作为 `CURLSH *` 暴露给外部 API。

理念是，这个结构可以有自己的缓存和池的版本集，然后通过在 `CURLOPT_SHARE` 选项中提供这个结构，那些特定的 `Curl_easy`s 就会使用这个共享句柄所持有的缓存/池。

然后，可以单独的 `Curl_easy` 结构被设置为共享特定的东西，否则它们不会共享，比如 cookies。

`Curl_share` 结构目前可以存储 cookies、DNS 缓存和 SSL 会话缓存。

## CookieInfo

这是主要的 cookie 结构。它包含所有已知的 cookies 和相关信息。即使它们被添加到多处理句柄中，每个 `Curl_easy` 也有自己的私有 `CookieInfo`。可以通过使用共享 API 使它们共享 cookies。
