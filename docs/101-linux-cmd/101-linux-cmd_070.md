# `service` 命令

Service 尽可能在尽可能可预测的环境中运行 System V init 脚本，移除大多数环境变量，并将当前工作目录设置为 /。

SCRIPT 参数指定一个位于 /etc/init.d/ 的 System V init 脚本。COMMAND 的支持值取决于调用的脚本，服务将未经修改地将 COMMAND 和 OPTIONS 传递给 init 脚本。所有脚本至少应支持启动和停止命令。作为一个特殊情况，如果 COMMAND 是 --full-restart，脚本将运行两次，首先使用停止命令，然后使用启动命令。

COMMAND 至少可以是 start、stop、status 和 restart。

`service --status-all` 运行所有 init 脚本，按字母顺序，使用 `status` 命令

示例：

1.  检查所有运行服务的状态：

```sh
      service --status-all 

```

1.  运行脚本

```sh
      service SCRIPT-Name start 

```

1.  一个更通用的命令：

```sh
      service [SCRIPT] [COMMAND] [OPTIONS] 

```
