# `column` 命令

`column` 命令用于将输入格式化为多列。它特别适用于创建基于文本的表格和改善命令输出的可读性。

## 命令语法

```sh
      column [options] [file]... 

```

## 命令选项

+   `-t`：自动确定列数并创建表格

+   `-s`：指定列分隔符（默认为空白字符）

+   `-n`：不合并多个相邻的分隔符

+   `-c`：以指定宽度输出列格式

+   `-x`：在行之前填充列

+   `-L`：将所有条目左对齐

+   `-R`：将所有条目右对齐

+   `-o`：指定表格输出的列分隔符

## 示例

1.  基本列格式化

```sh
      # Create a simple list and format it into columns
ls | column 

```

1.  从分隔数据创建表格

```sh
      # Format /etc/passwd entries into a neat table
cut -d: -f1,6,7 /etc/passwd | column -t -s: 

```

1.  格式化命令输出

```sh
      # Display mount information in a clean tabular format
mount | column -t 

```

1.  自定义列分隔符

```sh
      # Format CSV data with custom separatorecho -e "Name,Age,City\nJohn,25,NYC\nJane,30,LA" | column -t -s,

Name  Age  City
John  25   NYC
Jane  30   LA 

```

1.  左对齐表格

```sh
      # Create a left-aligned table from space-separated data
ps aux | head -n 5 | column -t -L 

```

> **注意：** 选项如 `-L` 和 `-R` 可能并非所有 Linux 发行版中都可用（主要是在 GNU `column` 中）。

# 将文件格式化为列（例如，data.txt）

column -t -s, data.txt

## 更多信息

+   `column` 命令是 `util-linux` 软件包的一部分

+   它在 shell 脚本中用于格式化输出特别有用

+   可以处理文件输入和标准输入（stdin）

+   与其他文本处理命令如 `cut`、`sort` 和 `grep` 一起使用效果良好

## 相关链接

+   `cut` 命令 - 从文件中删除部分内容

+   `sort` 命令 - 对文本文件的行进行排序

+   `paste` 命令 - 合并文件的行

+   `tr` 命令 - 转换或删除字符

## 进一步阅读

+   `man column` - `column` 命令的手册页面

+   [GNU Coreutils](https://www.gnu.org/software/coreutils/)
