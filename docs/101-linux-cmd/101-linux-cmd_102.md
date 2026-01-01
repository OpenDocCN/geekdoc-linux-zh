# `find` 命令

`find` 命令是 Linux 中最强大的实用工具之一，它允许您根据名称、大小、修改时间、权限等多种条件搜索文件和目录。

## 基本语法

```sh
      find [path] [options] [expression] 

```

**简单来说：**

> “从 `[path]` 开始搜索，应用 `[options or filters]`，然后对结果执行 `[action]`。”

示例：

```sh
      find /home/user -name "*.log" 

```

这将在 `/home/user` 下搜索所有 `.log` 文件。

## 常见用例

### 1. 通过精确名称搜索文件

```sh
      find ./directory1 -name sample.txt 

```

在 `directory1` 和其所有子目录中查找 `sample.txt`。

不区分大小写的搜索：

```sh
      find ./directory1 -iname "sample.txt" 

```

### 2. 通过模式（通配符）搜索文件

查找 `directory1` 下的所有 `.txt` 文件。

```sh
      find ./directory1 -name "*.txt" 

```

更多示例：

```sh
      find /var/log -name "*.log"
find /etc -name "conf*" 

```

### 3. 通过名称查找目录

```sh
      find / -type d -name test 

```

列出从根目录 `/` 开始的所有名为 `test` 的目录。

### 4. 查找空文件和目录

```sh
      find . -empty 

```

在当前文件夹中查找空文件和目录。

仅查找空文件：

```sh
      find . -type f -empty 

```

### 5. 通过修改或访问时间查找文件

| 选项 | 描述 |
| --- | --- |
| `-mtime n` | n 天前修改 |
| `-atime n` | n 天前访问 |
| `-ctime n` | n 天前更改 |
| `-mmin n` | n 分钟前修改 |

示例：

查找过去两天内修改的文件。

```sh
      find /var/log -mtime -2 

```

查找过去一小时内修改的文件。

```sh
      find . -mmin -60 

```

### 6. 通过大小查找文件

| 表达式 | 含义 |
| --- | --- |
| `+n` | 大于 n |
| `-n` | 小于 n |
| `n` | 精确等于 n |

示例：

查找大于 100 MB 的文件。

```sh
      find / -size +100M 

```

查找小于 10 KB 的文件。

```sh
      find . -size -10k 

```

### 7. 查找比另一个文件修改时间更近的文件

```sh
      find . -newer reference.txt 

```

列出在 `reference.txt` 之后修改的文件。

### 8. 通过类型查找文件

| 类型 | 描述 |
| --- | --- |
| `f` | 普通文件 |
| `d` | 目录 |
| `l` | 符号链接 |
| `b` | 块设备 |
| `c` | 字符设备 |

示例：

查找所有块设备。

```sh
      find /dev -type b 

```

### 9. 通过权限查找文件

查找权限精确为 644 的文件。

```sh
      find /var/www -type f -perm 644 

```

查找任何人可执行的文件。

```sh
      find /usr -type f -perm /111 

```

### 10. 执行找到的文件上的命令

您可以使用 `-exec` 选项对每个找到的文件运行命令：

```sh
      find . -type f -name "*.log" -exec rm -f {} \; 

```

删除当前目录下的所有 `.log` 文件。

**替代方案（更快）：**

```sh
      find . -type f -name "*.log" | xargs rm -f 

```

### 11. 对找到的文件执行命令

您可以将过滤器组合在一起：

```sh
      find /var/log -name "*.log" -size +50M -mtime -2 

```

查找大于 50 MB 且在过去两天内修改的 `.log` 文件。

### 12. 仅打印文件名（静默输出）

```sh
      find /etc -type f -name "*.conf" -print 

```

`-print` 标志确保输出显示（在大多数系统中是默认设置）。

## 快速参考表

| 任务 | 命令 |
| --- | --- |
| 查找具有特定名称的文件 | `find . -name filename.txt` |
| 不区分大小写的搜索 | `find . -iname filename.txt` |
| 仅查找目录 | `find . -type d` |
| 查找空文件 | `find . -type f -empty` |
| 查找大于 1 GB 的文件 | `find . -size +1G` |
| 查找昨天修改的文件 | `find . -mtime -1` |
| 删除 `.tmp` 文件 | `find . -name "*.tmp" -delete` |
| 搜索并执行 | `find . -name "*.log" -exec gzip {} \;` |

## 获取帮助

要查看 `find` 命令的完整指南，请运行：

```sh
      man find 

```
