# `cat`命令

* * *

`cat`命令允许我们创建单个或多个文件，查看文件内容，或将文件连接并重定向输出到终端或文件。

“cat”代表“连接”，它是 Linux 终端中最常用的命令之一。

### 使用示例：

1.  要在终端中显示文件内容：

```sh
      cat <specified_file_name> 

```

1.  在终端中显示多个文件的内容：

```sh
      cat file1 file2 ... 

```

1.  使用 cat 命令创建文件：

```sh
      cat > file_name 

```

1.  要显示当前目录中具有相同文件类型的所有文件：

```sh
      cat *.<filetype> 

```

1.  要显示当前目录中所有文件的内容：

```sh
      cat * 

```

1.  将给定文件的内容放入另一个文件中：

```sh
      cat old_file_name > new_file_name 

```

1.  使用带有`more`和`less`选项的 cat 命令：

```sh
      cat filename | more
cat filename | less 

```

1.  将 file1 的内容追加到 file2：

```sh
      cat file1 >> file2 

```

1.  将两个文件合并到一个新文件中：

```sh
      cat file1_name file2_name merge_file_name 

```

1.  一些 cat 的实现，使用选项-n，可以显示行号：

```sh
      cat -n file1_name file2_name > new_numbered_file_name 

```

### 语法：

```sh
      cat [OPTION] [FILE]... 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-A` | `--show-all` | 等同于 -vET |
| `-b` | `--number-nonblank` | 对非空输出行进行编号，覆盖-n |
| `-e` | - | 等同于 -vE |
| `-T` | - | 使用`cat`命令打开的文件中显示制表符分隔的行。 |
| `-E` | - | 在每个文件的末尾显示$。 |
| `-E` | - | 显示带行号的文件。 |
| `-n` | `--number` | 对所有输出行进行编号 |
| `-s` | `--squeeze-blank` | 抑制重复的空输出行 |
| `-u` | - | （忽略） |
| `-v` | `--show-nonprinting` | 使用^和 M-符号，除了 LFD 和 TAB |
| - | `--help` | 显示此帮助信息并退出 |
| - | `--version` | 输出版本信息并退出 |

* * *

# `tac`命令

`tac`是 Linux 命令，允许你逐行查看文件，从最后一行开始。（`tac`不会反转每行的内容，只会反转行显示的顺序。）它是以`cat`命令命名的。

### 使用示例：

1.  要在终端中显示文件内容：

```sh
      tac <specified_file_name> 

```

1.  此选项在之前而不是之后附加分隔符。

```sh
      tac -b concat_file_name tac_example_file_name 

```

1.  此选项将分隔符解释为正则表达式。

```sh
      tac -r concat_file_name tac_example_file_name 

```

1.  此选项使用 STRING 作为分隔符而不是换行符。

```sh
      tac -s concat_file_name tac_example_file_name 

```

1.  此选项将显示帮助文本并退出。

```sh
      tac --help 

```

1.  此选项将输出版本信息并退出。

```sh
      tac --version 

```

### 语法：

```sh
      tac [OPTION]... [FILE]... 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-b` | `--before` | 在之前而不是之后附加分隔符 |
| `-r` | `--regex` | 将分隔符解释为正则表达式 |
| `-s` | `--separator=STRING` | 使用 STRING 作为分隔符而不是换行符 |
| - | `--help` | 显示此帮助信息并退出 |
| - | `--version` | 输出版本信息并退出 |
