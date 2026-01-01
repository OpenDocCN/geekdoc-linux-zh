# `netstat` 命令

术语 `netstat` 代表网络统计。用通俗易懂的话来说，`netstat` 命令显示当前的网络连接、网络协议统计信息以及各种其他接口。

检查您的电脑上是否有 `netstat`：

```sh
      netstat –v 

```

如果您的电脑上没有安装 `netstat`，您可以使用以下命令进行安装：

```sh
      sudo apt install net-tools 

```

### 您可以使用以下 `netstat` 命令进行一些用例：

+   带有 `-nr` 标志的 `Netstat` 命令在终端上显示路由表详情。

示例：

```sh
      netstat  -nr 

```

+   带有 `-i` 标志的 `Netstat` 命令显示当前配置的网络接口的统计信息。此命令将显示文件 `foo.txt` 的前 10 行。

示例：

```sh
      netstat  -i 

```

+   带有 `-tunlp` 标志的 `Netstat` 命令将列出网络、它们的当前状态以及它们关联的端口。

示例：

```sh
      netstat -tunlp 

```

+   您可以使用 `-at` 与 `netstat` 一起使用来获取所有 TCP 端口连接列表。

```sh
      netstat  -at 

```

+   您可以使用 `-au` 与 `netstat` 一起使用来获取所有 UDP 端口连接列表。

```sh
      netstat  -au 

```

+   您可以使用 `-l` 与 `netstat` 一起使用来获取所有活动的连接列表。

```sh
      netstat  -l 

```
