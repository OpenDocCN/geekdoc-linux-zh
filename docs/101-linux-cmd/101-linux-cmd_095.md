# `tree` 命令

Linux 中的 `tree` 命令递归地以树状结构列出目录。每个列表根据其相对于树根的深度进行缩进。

### 示例：

1.  显示当前目录的树表示。

```sh
      tree 

```

1.  -L NUMBER 限制递归深度以避免显示非常深的树。

```sh
      tree -L 2 / 

```

### 语法：

```sh
      tree  [-acdfghilnpqrstuvxACDFQNSUX]  [-L  level [-R]] [-H baseHREF] [-T title]
      [-o filename] [--nolinks] [-P pattern] [-I  pattern]  [--inodes]
      [--device] [--noreport] [--dirsfirst] [--version] [--help] [--filelimit #]
      [--si] [--prune] [--du] [--timefmt  format]  [--matchdirs]  [--from-file]
      [--] [directory ...] 

```

### 其他标志及其功能：

| **标志** | **描述** |
| :-- | :-- |
| `-a` | 打印所有文件，包括隐藏文件。 |
| `-d` | 只列出目录。 |
| `-l` | 跟随符号链接进入目录。 |
| `-f` | 打印每个列表的完整路径，而不仅仅是其基本名称。 |
| `-x` | 不要跨越文件系统。 |
| `-L #` | 限制递归深度为 #。 |
| `-P REGEX` | 递归，但只列出与 REGEX 匹配的文件。 |
| `-I REGEX` | 递归，但不列出与 REGEX 匹配的文件。 |
| `--ignore-case` | 在模式匹配时忽略大小写。 |
| `--prune` | 从输出中剪除空目录。 |
| `--filelimit #` | 忽略包含超过 # 个文件的目录。 |
| `-o FILE` | 将标准输出重定向到 FILE。 |
| `-i` | 不输出缩进。 |
