# SCP 和 SFTP

如果 curl 在构建时使用了先决的第三方库：[libssh2](https://www.libssh2.org/)，[libssh](https://www.libssh.org/)或[wolfSSH](https://www.wolfssl.com/products/wolfssh/)，则支持 SCP 和 SFTP 协议。

SCP 和 SFTP 都是建立在 SSH 之上的协议，SSH 是一个安全且加密的数据协议，与 TLS 类似，但在几个重要方面有所不同。例如，SSH 不使用任何类型的证书，而是使用公钥和私钥。当正确使用时，SSH 和 TLS 都提供强大的加密和安全的传输。

SCP 协议通常被认为是两者中的“黑羊”，因为它不可移植，通常只在 Unix 系统之间工作。

## URL

SFTP 和 SCP 的 URL 与其他 URL 类似，您可以使用这些协议下载文件，就像使用其他协议一样：

```sh
curl sftp://example.com/file.zip -u user
```

和：

```sh
curl scp://example.com/file.zip -u user
```

SFTP（但不是 SCP）支持在 URL 以斜杠结尾时获取文件列表：

```sh
curl sftp://example.com/ -u user
```

注意，这两个协议都与“用户”一起工作，您不能匿名或使用标准通用名称请求文件。大多数系统要求用户进行认证，如下所述。

当从 SFTP 或 SCP URL 请求文件时，提供的文件路径被认为是远程服务器上的绝对路径，除非您明确要求相对于用户主目录的路径。您可以通过确保路径以`/~/`开头来实现这一点。这与 FTP URL 的工作方式正好相反，并且是用户中常见的混淆原因。

对于用户`daniel`，要从他的主目录传输`todo.txt`，看起来可能像这样：

```sh
curl sftp://example.com/~/todo.txt -u daniel
```

## 认证

使用 curl 对 SSH 服务器进行认证（当您指定 SCP 或 SFTP URL 时）是这样进行的：

1.  curl 连接到服务器并了解该服务器提供的认证方法

1.  curl 会依次尝试提供的方法，直到找到一个有效的方法或者都失败

如果服务器提供公钥认证，curl 会尝试使用您在主目录的`.ssh`子目录中找到的公钥。在这样做的时候，您仍然需要告诉 curl 在服务器上使用哪个用户名。例如，用户‘john’在远程 SFTP 服务器`sftp.example.com`上列出其主目录的条目：

```sh
curl -u john: sftp://sftp.example.com/
```

如果 curl 由于任何原因无法使用公钥进行认证，它将尝试使用用户名加密码（如果服务器允许并且凭证通过命令行传递）。

例如，上面的同一个用户在远程系统上的密码是`RHvxC6wUA`，可以通过以下方式使用 SCP 下载文件：

```sh
curl -u john:RHvxC6wUA -O scp://ssh.example.com/file.tar.gz
```

## 已知主机

一个安全的网络客户端需要确保远程主机正是它认为正在与之通信的主机。基于 TLS 的协议通过客户端验证服务器的证书来实现。

在 SSH 协议中，没有服务器证书，而是每个服务器都可以提供其唯一的密钥。与 TLS 不同，SSH 没有证书颁发机构或类似的东西，因此客户端只需确保主机的密钥与其通过其他方式已知（应该看起来）的密钥匹配即可。

密钥匹配通常是通过密钥的哈希值来完成的，客户端存储已知服务器哈希的文件通常被称为`known_hosts`，并放置在专用的 SSH 目录中。在 Linux 系统中，这通常称为`~/.ssh`。

当 curl 连接到 SFTP 和 SCP 主机时，它会确保主机的密钥哈希已经存在于已知主机文件中，否则它会拒绝继续操作，因为它无法信任该服务器是正确的。一旦正确的哈希存在于`known_hosts`中，curl 就可以执行传输。

要强制 curl 跳过检查和遵守`known_hosts`文件，您可以使用`-k / --insecure`命令行选项。您必须非常小心地使用此选项，因为它使得中间人攻击不被检测到成为可能。
