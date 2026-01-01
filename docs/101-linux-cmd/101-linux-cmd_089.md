# `ssh` 命令

Linux 中的 `ssh` 命令代表“安全壳”。这是一个用于安全连接到远程服务器/系统的协议。与客户端之间以加密形式传输数据，因此 ssh 更安全。ssh 在 TCP/IP 端口 22 上运行。

### 示例：

1.  使用不同的端口号进行 SSH 连接：

```sh
      ssh test.server.com -p 3322 

```

1.  - 使用私钥连接到远程服务器 `ssh`？

```sh
      ssh -i private.key user_name@host 

```

1.  - 使用 `ssh` 指定不同的用户名

```sh
      ssh -l alternative-username sample.ssh.com 

```

### 语法：

```sh
      ssh user_name@host(IP/Domain_Name) 

```

```sh
      ssh -i private.key user_name@host 

```

```sh
      ssh sample.ssh.com  ls /tmp/doc 

```

### 其他标志及其功能：

| **标志** | **描述** |
| :-- | :-- |
| `-1` | 强制 ssh 只使用 SSH-1 协议。 |
| `-2` | 强制 ssh 只使用 SSH-2 协议。 |
| `-4` | 仅允许 IPv4 地址。 |
| `-A` | 启用身份验证代理连接转发。 |
| `-a` | 禁用身份验证代理连接转发。 |
| `-B bind_interface` | 在尝试连接到目标主机之前绑定到 bind_interface 的地址。这在具有多个地址的系统上才有用。 |
| `-b bind_address` | 在本地机器上使用 bind_address 作为连接的源地址。这在具有多个地址的系统上才有用。 |
| `-C` | 对所有数据（包括 stdin、stdout、stderr 以及转发 X11 和 TCP 连接的数据）进行压缩，以加快数据传输。 |
| `-c cipher_spec` | 选择用于加密会话的加密规范。 |
| `-D [bind_address:]port` | 动态应用层端口转发。这为本地侧的端口分配一个套接字进行监听。当连接到此端口时，连接将通过安全通道进行转发，然后使用应用程序协议确定从远程机器连接到何处。 |
| `-E log_file` | 将调试日志附加到标准错误。 |
| `-e escape_char` | 设置与 pty 会话的转义字符（默认：‘~’）。转义字符仅在行首被识别。跟在点（‘.’）后面的转义字符关闭连接；跟在控制-Z 后挂起连接；跟在自己后面发送一次转义字符。将字符设置为“none”将禁用任何转义，并使会话完全透明。 |
| `-F configfile` | 指定每个用户的配置文件。每个用户的默认配置文件是 ~/.ssh/config。 |
| `-f` | 请求 ssh 在命令执行前进入后台。 |
| `-G` | 导致 ssh 在评估 Host 和 Match 块后打印其配置并退出。 |
| `-g` | 允许远程主机连接到本地转发端口。 |
| `-I pkcs11` | 指定 ssh 应用于与提供密钥的 PKCS#11 令牌通信的共享库。 |
| `-i identity_file` | 从该文件读取用于公钥认证的标识密钥（私钥）。 |
| `-J [user@]host[:port]` | 首先通过 ssh 连接到 pjump 主机[(/iam/jump-host)]，然后从那里建立到最终目标的 TCP 转发连接以连接到目标主机。 |
| `-K` | 启用基于 GSSAPI 的身份验证和转发（委派）GSSAPI 凭证到服务器。 |
| `-k` | 禁用将 GSSAPI 凭证转发（委派）到服务器。 |
| `-L [bind_address:]port:host:hostport`, `-L [bind_address:]port:remote_socket`, `-L local_socket:host:hostport`, `-L local_socket:remote_socket` | 指定到本地（客户端）主机上给定 TCP 端口或 Unix 套接字的连接应转发到远程端的主机和端口，或 Unix 套接字。这是通过在本地端分配一个套接字来监听 TCP 端口，可选地绑定到指定的 bind_address，或 Unix 套接字来实现的。每当连接到本地端口或套接字时，连接将通过安全通道转发，并从远程机器建立到主机端口 hostport 或 Unix 套接字 remote_socket 的连接。 |
| `-l login_name` | 指定在远程机器上登录的用户。 |
| `-M` | 将 ssh 客户端置于“主”模式以进行连接共享。多个-M 选项将 ssh 置于“主”模式，但在每个更改多路复用状态的操作（例如打开新会话）之前需要使用 ssh-askpass 进行确认。 |
| `-m mac_spec` | 以逗号分隔的 MAC（消息认证代码）算法列表，按优先级指定。 |
| `-N` | 不执行远程命令。这对于仅转发端口很有用。 |
| `-n` | 防止从 stdin 读取。 |
| `-O ctl_cmd` | 控制活动连接多路复用主进程。当指定-O 选项时，ctl_cmd 参数将被解释并传递给主进程。有效的命令有：“check”（检查主进程是否正在运行）、“forward”（请求转发而不执行命令）、“cancel”（取消转发）、“exit”（请求主进程退出）和“stop”（请求主进程停止接受进一步的多路复用请求）。 |
| `-o` | 可以用于以配置文件中使用的格式给出选项。这对于指定没有单独命令行标志的选项很有用。 |
| `-p`, `--port PORT` | 连接到远程主机的端口号。 |
| `-Q query_option` | 对 ssh 进行查询，以获取指定版本 2 支持的计算法。可用的功能包括：cipher（支持的对称加密算法）、cipher-auth（支持认证加密的对称加密算法）、help（与-Q 标志一起使用的支持查询术语）、mac（支持的消息完整性代码）、kex（密钥交换算法）、kex-gss（GSSAPI 密钥交换算法）、key（密钥类型）、key-cert（证书密钥类型）、key-plain（非证书密钥类型）、key-sig（所有密钥类型和签名算法）、protocol-version（支持的 SSH 协议版本）和 sig（支持的签名算法）。或者，可以使用 ssh_config(5)或 sshd_config(5)中任何接受算法列表的关键字作为相应 query_option 的别名。 |
| `-q` | 静音模式。导致大多数警告和诊断信息被抑制。 |
| `-R [bind_address:]port:host:hostport, -R [bind_address:]port:local_socket, -R remote_socket:host:hostport, -R remote_socket:local_socket, -R [bind_address:]port` | 指定连接到远程（服务器）主机上的给定 TCP 端口或 Unix 套接字应转发到本地端。 |
| `-S ctl_path` | 指定连接共享的控制套接字的位置，或字符串“none”以禁用连接共享。 |
| `-s` | 可用于请求在远程系统上调用子系统。子系统便于将 SSH 作为其他应用程序（例如 sftp(1)）的安全传输使用。子系统作为远程命令指定。 |
| `-T` | 禁用伪终端分配。 |
| `-t` | 强制分配伪终端。这可以用于在远程机器上执行任意基于屏幕的程序，这非常有用，例如在实现菜单服务时。多个 -t 选项强制分配 tty，即使 ssh 没有本地 tty。 |
| `-V` | 显示版本号。 |
| `-v` | 详细模式。在建立连接时回显其正在执行的所有操作。在调试连接失败时非常有用。 |
| `-W host:port` | 请求将客户端的标准输入和输出通过安全通道转发到主机上的端口。隐含 -N、-T、ExitOnForwardFailure 和 ClearAllForwardings，尽管这些可以在配置文件或使用 -o 命令行选项中覆盖。 |
| `-w local_tun[remote_tun]` | 请求使用指定的 tun 设备在客户端（local_tun）和服务器（remote_tun）之间进行隧道设备转发。设备可以通过数字 ID 或关键字“any”指定，后者使用下一个可用的隧道设备。如果未指定 remote_tun，则默认为“any”。如果未设置隧道指令，则将其设置为默认隧道模式，即“点对点”。如果需要不同的隧道转发模式，则应在 -w 之前指定。 |
| `-X` | 启用 X11 转发（GUI 转发）。 |
| `-x` | 禁用 X11 转发（GUI 转发）。 |
| `-Y` | 启用受信任的 X11 转发。 |
| `-y` | 使用 syslog 系统模块发送日志信息。默认情况下，此信息发送到 stderr。 |
