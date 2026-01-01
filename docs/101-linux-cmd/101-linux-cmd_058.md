# `reboot` 命令

`reboot` 命令用于重启 Linux 系统。然而，它需要使用 `sudo` [命令](https://github.com/bobbyiliev/101-linux-commands/blob/main/ebook/en/content/051-the-sudo-command.md) 提升权限。通常在系统或网络更新后需要使用此命令。

## 语法

```sh
      reboot [OPTIONS...] 

```

### 选项

+   **–help**：此选项打印简短的帮助文本并退出。

+   **-halt**：此命令将停止机器。

+   **-w**，**–wtmp-only**：此选项仅写入 wtmp 关闭条目，实际上不会停止、关机或重启。

### 示例

1.  基本用法。主要用于重启，无需任何其他详细信息

```sh
      $ sudo reboot 

```

然而，也可以使用带有 `-r` 选项的 `shutdown` 命令

```sh
      $ sudo shutdown -r now 

```

**注意**：reboot、halt 和关机在语法和效果上几乎相同。运行每个命令的 –-help 以查看详细信息。

1.  `reboot` 命令的使用有限，而 `shutdown` 命令被用来替代重启命令以满足更高级的重启和关机需求。其中一种情况是计划重启。语法如下

```sh
      $ sudo shutdown –r [TIME] [MESSAGE] 

```

这里的时间有各种格式。最简单的一种是 `now`，已在上一节中列出，告诉系统立即重启。其他有效的格式包括 +m，其中 m 是我们等待重启所需的分钟数，以及 HH:MM，它指定了 24 小时制的时间。

**示例：2 分钟后重启系统**

```sh
      $ sudo shutdown –r +2 

```

**示例：凌晨 3:00 计划重启**

```sh
      $ sudo shutdown –r 03:00 

```

1.  取消重启。通常发生在某人想要取消计划重启的情况下

**语法**

```sh
      $ sudo shutdown –c [MESSAGE] 

```

**用法**

```sh
      $sudo shutdown -c "Scheduled reboot cancelled because the chicken crossed the road" 

```

1.  显示系统重启的历史记录 **语法**

```sh
      $ last reboot 

```
