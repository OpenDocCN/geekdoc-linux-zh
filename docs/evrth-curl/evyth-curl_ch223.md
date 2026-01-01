# Opensocket 和 closesocket

有时你可能会遇到这样的情况，你希望你的应用程序能够更精确地控制 libcurl 操作所使用的套接字库。libcurl 提供了一对回调，用于替换 libcurl 对`socket()`的调用以及随后对相同文件描述符的`close()`操作。

## 提供一个文件描述符

通过设置`CURLOPT_OPENSOCKETFUNCTION`回调，你可以提供一个自定义函数，以便 libcurl 返回一个文件描述符：

```sh
curl_easy_setopt(handle, CURLOPT_OPENSOCKETFUNCTION, opensocket_callback);
```

`opensocket_callback`函数必须匹配以下原型：

```sh
curl_socket_t opensocket_callback(void *clientp,
                                  curlsocktype purpose,
                                  struct curl_sockaddr *address);
```

回调将`clientp`作为第一个参数，它是一个简单的不可见指针，你使用`CURLOPT_OPENSOCKETDATA`设置它。

其他两个参数传递的数据用于标识套接字将被用于什么*目的*和*地址*。*目的*是一个 typedef，其值为`CURLSOCKTYPE_IPCXN`或`CURLSOCKTYPE_ACCEPT`，用于标识套接字是在哪种情况下创建的。在 libcurl 用于接受 FTP 主动模式下的传入 FTP 连接时，“接受”情况发生，而在 libcurl 创建套接字用于其自己的出站连接的所有其他情况下，传递的值是*IPCXN*。 

*地址*指针指向一个`struct curl_sockaddr`，它描述了为创建此套接字而创建的网络目标 IP 地址。例如，你的回调可以使用这些信息来白名单或黑名单特定的地址或地址范围。

如果你想在结构体中修改目标地址，以便提供某种网络过滤器或转换层，socketopen 回调也被明确允许。

回调应该返回一个文件描述符或`CURL_SOCKET_BAD`，这将导致 libcurl 内部发生不可恢复的错误，并从其 perform 函数返回`CURLE_COULDNT_CONNECT`。

如果你希望返回一个已经连接到服务器的文件描述符，那么你也必须设置 sockopt 回调并确保它返回正确的返回值。

`curl_sockaddress`结构看起来像这样：

```sh
struct curl_sockaddr {
  int family;
  int socktype;
  int protocol;
  unsigned int addrlen;
  struct sockaddr addr;
};
```

## Socket 关闭回调

当然，打开套接字的对应回调是关闭套接字。通常，当你提供一种自定义方式来提供文件描述符时，你希望提供自己的清理版本：

```sh
curl_easy_setopt(handle, CURLOPT_CLOSESOCKETFUNCTION,
                 closesocket_callback);
```

`closesocket_callback`函数必须匹配以下原型：

```sh
int closesocket_callback(void *clientp, curl_socket_t item);
```
