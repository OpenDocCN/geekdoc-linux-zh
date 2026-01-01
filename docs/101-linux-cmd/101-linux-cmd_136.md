# `wall`命令

`wall`命令（简称*write all*）用于向 Linux 系统上所有已登录用户发送消息。它通常由系统管理员用于广播重要信息，例如计划维护或紧急公告。

## 语法

```sh
      $wall [options] [message] 

```

如果命令行上未提供[消息]，wall 将从标准输入读取，直到接收到文件结束字符（Ctrl+D）。

## 选项

| 选项 | 描述 |
| --- | --- |
| -n | 抑制显示发送者信息的横幅（显示谁发送了消息），只显示消息文本。 |
| -t [秒数] | 设置超时时间（秒）。`wall`将尝试在此期间向用户的终端写入，如果失败则放弃。 |

## 示例：

### 1. 向所有用户广播消息

此命令直接向所有已登录用户发送消息。

```sh
      $ wall "The system will shut down in 10 minutes. Please save your work." 

```

### 输出（在其他用户的终端上）：

```sh
      Broadcast message from your_username@hostname (pts/0) (Sat Oct  4 19:50:00 2025):

The system will shut down in 10 minutes. Please save your work. 

```

### 2. 从文本文件广播消息

你可以将文件内容重定向到用作消息的内容。

### 消息.txt 的内容：

```sh
      System maintenance will begin shortly.
Connections may be temporarily unstable. 

```

### 命令：

```sh
      $ wall < message.txt 

```

### 3. 从标准输入发送多行消息

如果你运行 wall 命令而不带消息，你可以在终端中直接输入多行消息。完成输入后按 Ctrl+D 发送。

```sh
      $ wall
The server is now back online.
All services are running normally.
<Ctrl+D> 

```

### 输出（在其他用户的终端上）：

```sh
      Broadcast message from your_username@hostname (pts/0) (Sat Oct  4 19:52:00 2025):

The server is now back online.
All services are running normally. 

```
