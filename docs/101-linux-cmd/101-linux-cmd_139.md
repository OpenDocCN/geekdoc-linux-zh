# `journalctl` 命令

`journalctl` 命令用于查看和查询 systemd 日志，它以结构化和索引化的格式收集和存储系统日志。它是现代 Linux 发行版中查看系统日志的主要工具。

## 语法

```sh
      journalctl [options] [matches] 

```

## 选项

一些流行的选项标志包括：

```sh
      -f          Follow journal (like tail -f)
-u [unit]   Show logs for specific unit/service
-p [level]  Filter by priority level (0-7)
-S [time]   Show entries since specified time
-U [time]   Show entries until specified time
-b          Show logs from current boot
-k          Show kernel messages only
-r          Reverse output (newest first)
-n [lines]  Show last N lines
--no-pager  Don't pipe output to pager
-x          Add explanatory help texts
-o [format] Output format (json, short, verbose, etc.)
--disk-usage Show current disk usage
--vacuum-size=[size] Remove logs to reduce size
--vacuum-time=[time] Remove logs older than time 

```

## 优先级

```sh
      0  Emergency (emerg)
1  Alert (alert)
2  Critical (crit)
3  Error (err)
4  Warning (warning)
5  Notice (notice)
6  Informational (info)
7  Debug (debug) 

```

## 示例

1.  查看所有日志条目

```sh
      journalctl 

```

1.  跟踪实时日志条目

```sh
      journalctl -f 

```

1.  显示特定服务的日志

```sh
      journalctl -u nginx 

```

1.  显示上次启动以来的日志

```sh
      journalctl -b 

```

1.  显示上次启动以来的日志

```sh
      journalctl -b -1 

```

1.  显示内核消息

```sh
      journalctl -k 

```

1.  显示特定时间的日志

```sh
      journalctl --since "2024-01-01 00:00:00" 

```

1.  显示上小时的日志

```sh
      journalctl --since "1 hour ago" 

```

1.  显示时间间隔内的日志

```sh
      journalctl --since "2024-01-01" --until "2024-01-02" 

```

1.  仅显示错误和关键消息

```sh
      journalctl -p err 

```

1.  显示最后 50 行

```sh
      journalctl -n 50 

```

1.  跟踪特定服务的日志

```sh
      journalctl -u ssh -f 

```

1.  以 JSON 格式显示日志

```sh
      journalctl -o json 

```

1.  显示磁盘使用情况

```sh
      journalctl --disk-usage 

```

1.  删除旧日志以释放空间

```sh
      journalctl --vacuum-size=100M 

```

1.  删除 2 周前的旧日志

```sh
      journalctl --vacuum-time=2weeks 

```

1.  显示带解释的日志

```sh
      journalctl -x 

```

1.  显示特定进程 ID 的日志

```sh
      journalctl _PID=1234 

```

1.  显示特定用户的日志

```sh
      journalctl _UID=1000 

```

1.  以倒序显示

```sh
      journalctl -r 

```

## 时间指定

您可以使用各种时间格式：

+   `"2024-01-01 12:00:00"`

+   `"昨天"`

+   `"今天"`

+   `"1 小时前"`

+   `"30 分钟前"`

+   `"2 天前"`

## 输出格式

```sh
      short      Default format
verbose    All available fields
json       JSON format
json-pretty Pretty-printed JSON
export     Binary export format
cat        Very short format 

```

## 用例

+   故障排除系统问题

+   监控服务行为

+   安全审计

+   性能分析

+   调试系统问题

+   跟踪用户活动

## 重要注意事项

+   日志文件存储在 `/var/log/journal/` 或 `/run/log/journal/`

+   需要适当的权限来查看系统日志

+   随着时间的推移可能会消耗大量的磁盘空间

+   使用真空选项来管理日志大小

+   持久化日志需要适当的配置

`journalctl` 命令对于基于 systemd 的 Linux 发行版的系统管理和故障排除至关重要。

想要更多详细信息，请查看手册：`man journalctl`
