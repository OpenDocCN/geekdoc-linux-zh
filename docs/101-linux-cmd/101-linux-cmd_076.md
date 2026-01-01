# `scp` 命令

SCP（安全复制）是一个命令行实用程序，允许您在两个位置之间安全地复制文件和目录。

文件和密码都进行了加密，这样任何窃听流量的人都不会得到任何敏感信息。

### 复制文件或目录的不同方式：

+   从本地系统到远程系统。

+   从远程系统到本地系统。

+   从本地系统复制到两个远程系统。

### 示例：

1.  要将文件从本地系统复制到远程系统：

```sh
      scp /home/documents/local-file root@{remote-ip-address}:/home/ 

```

1.  要将文件从远程系统复制到本地系统：

```sh
      scp root@{remote-ip-address}:/home/remote-file /home/documents/ 

```

1.  要在本地系统之间复制两个远程系统中的文件。

```sh
      scp root@{remote1-ip-address}:/home/remote-file root@{remote2-ip-address}/home/ 

```

1.  通过跳转主机服务器复制文件。

```sh
      scp /home/documents/local-file -oProxyJump=<jump-host-ip> root@{remote-ip-address}/home/ 

```

在某些机器上较新版本的 scp 中，您可以使用上述命令并带有 `-J` 标志。

```sh
      scp /home/documents/local-file -J <jump-host-ip> root@{remote-ip-address}/home/ 

```

### 语法：

```sh
      scp [OPTION] [user@]SRC_HOST:]file1 [user@]DEST_HOST:]file2 

```

+   `OPTION` - scp 选项，如加密、ssh 配置、ssh 端口、限制、递归复制等。

+   `[用户@]源主机:]文件 1` - 源文件

+   `[用户@]目标主机:]文件 2` - 目标文件

应使用绝对或相对路径指定本地文件，而远程文件名应包括用户和主机指定。

scp 提供了多种选项来控制其行为的各个方面。最常用的选项包括：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-P` | - | 指定远程主机的 ssh 端口。 |
| `-p` | - | 保留文件的修改时间和访问时间。 |
| `-q` | - | 如果您想抑制进度条和非错误消息，请使用此选项。 |
| `-C` | - | 此选项强制 scp 在发送到目标机器时压缩数据。 |
| `-r` | - | 这个选项告诉 scp 递归地复制目录。 |

### 在开始之前

`scp` 命令依赖于 `ssh` 进行数据传输，因此它需要在远程系统上进行 `ssh 密钥` 或 `密码` 认证。

`冒号 (:)` 是 scp 区分本地和远程位置的方式。

要能够复制文件，您必须在源文件上至少有读取权限，在目标系统上有写入权限。

在复制两个系统上具有相同名称和位置的文件时，请小心，`scp` 会无警告地覆盖文件。

在传输大文件时，建议在 `screen` 或 `tmux` 会话中运行 scp 命令。
