# `nohup` 命令

当 shell 退出（可能在退出 SSH 会话时），HUP（挂起）信号会被发送到其所有子进程，导致它们终止。如果你需要在退出 shell 后继续运行长时间运行的过程，你需要使用 `nohup` 命令。在任意命令前加上 `nohup` 会使得该命令对 HUP 信号具有 *免疫性*。此外，标准输入（STDIN）将被忽略，所有输出都会被重定向到本地文件 `./nohup.out`。

### 示例：

1.  将 `nohup` 应用于长时间运行的 Debian 升级：

```sh
      nohup apt-get -y upgrade 

```

### 语法：

```sh
      nohup COMMAND [ARG]...
nohup OPTION 

```
