# `tail` 命令

`tail` 命令打印文件的最后十行。

示例：

```sh
      tail filename.txt 

```

语法：

```sh
      tail [OPTION] [FILENAME] 

```

## 常见用例

### 获取特定数量的行：

使用 `-n` 选项和一个数字（应该是整数）的行数来显示。

示例：

```sh
      tail -n 10 foo.txt 

```

此命令将显示文件 `foo.txt` 的最后十行。

### 实时监控文件中的新条目

可以让 `tail` 输出你正在查看的文件中添加的任何新行。因此，如果向文件写入新行，它将立即显示在你的输出中。这可以通过使用 `--follow` 或 `-f` 选项来完成。这对于监控日志文件特别有用。

示例：

```sh
      tail -f foo.txt 

```

按 `Ctrl+C` 停止跟踪。

### 组合选项以进行日志监控

```sh
      tail -n 100 -f app.log      # Show last 100 lines, then follow
tail -f -s 2 slow.log       # Follow with 2-second refresh interval 

```

### 同时跟踪多个文件

```sh
      tail -f /var/log/nginx/access.log /var/log/nginx/error.log 

```

### 显示特定的字节范围

```sh
      tail -c 100 file.txt        # Last 100 bytes
tail -c +50 file.txt        # From byte 50 to end 

```

### 即使在轮换后也继续跟踪日志

```sh
      tail -F /var/log/app.log 

```

`-F` 选项对于监控由 logrotate 管理的日志至关重要。

### 跳过标题行

```sh
      tail -n +2 data.csv         # Start from line 2 (skip header)
tail -n +11 file.txt        # Start from line 11 onwards 

```

### 多个文件的静默模式

```sh
      tail -q -n 5 *.log          # No filename headers 

```

### 额外标志及其功能

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |

| `-c` | `--bytes=[+]NUM` | 输出最后 NUM 个字节；或使用 -c +NUM 以

输出每个文件从字节 NUM 开始的内容 |

| `-f` | `--follow[={name&#124;descriptor}]` | 随着文件增长输出附加数据；省略选项参数表示 'descriptor' |
| --- | --- | --- |
| `-F` |  | 与 --follow=name --retry 相同 |
| `-n` | `--lines=[+]NUM` | 输出最后 NUM 行，而不是最后 10 行；或使用 -n +NUM 以从行 NUM 开始输出 |

|  | `--max-unchanged-stats=N` | 使用 --follow=name 时，在 N（默认 5）次迭代后重新打开未更改大小的 FILE

以查看它是否已被取消链接或重命名

（这是轮换日志文件的通常情况）；

使用 inotify，此选项很少有用 |

|  | `--pid=PID` | 使用 -f 时，在进程 ID，PID 死亡后终止 |
| --- | --- | --- |
| `-q` | `--quiet, --silent` | 从不输出包含文件名的标题 |
|  | `--retry` | 如果文件不可访问，则继续尝试打开文件 |

| `-s` | `--sleep-interval=N` | 使用 -f 时，在迭代之间暂停大约 N 秒（默认 1.0）；

使用 inotify 和 --pid=P，检查进程 P 在

至少每 N 秒一次 |

| `-v` | `--verbose` | 总是输出包含文件名的标题 |
| --- | --- | --- |
| `-z` | `--zero-terminated` | 行分隔符是 NUL，而不是换行符 |
|  | `--help` | 显示此帮助信息并退出 |
|  | `--version` | 输出版本信息并退出 |

## 小贴士

+   使用 `-F` 而不是 `-f` 进行生产日志监控

+   与 `grep`、`awk` 或 `sed` 结合进行过滤监控

+   对于大文件，使用 `tail` 比在编辑器中打开要快得多

+   使用 `-n +N` 从特定行开始（对于跳过标题很有用）
