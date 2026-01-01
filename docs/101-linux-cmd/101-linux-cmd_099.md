# `sed` 命令

`sed` 命令代表流编辑器。流编辑器用于对输入流（文件或来自管道的输入）执行基本的文本转换。例如，它可以对文件执行许多功能，如搜索、查找和替换、插入或删除。虽然它在某些方面与允许脚本编辑的编辑器（如 `ed`）类似，但 `sed` 通过仅对输入（s）进行一次遍历来工作，因此效率更高。但它过滤文本在管道中的能力特别区别于其他类型的编辑器。

`sed` 命令最常用的用途是用于替换或查找和替换。通过使用 `sed`，即使不打开文件也可以编辑文件，这是一种更快的方法来在文件中查找和替换内容。它支持基本和扩展的正则表达式，允许您匹配复杂的模式。大多数 Linux 发行版都预装了 GNU `sed`。

### 示例：

1.  使用 `sed` 查找和替换字符串

```sh
      sed -i 's/{search_regex}/{replace_value}/g' input-file 

```

1.  对于递归查找和替换（与 `find` 一起使用）

> 有时您可能想要递归地搜索目录以查找包含字符串的文件，并在所有文件中替换该字符串。这可以通过使用 `find` 命令递归地查找目录中的文件并将文件名传递给 `sed` 来完成。以下命令将递归地搜索当前工作目录中的文件并将文件名传递给 `sed`。它将递归地搜索当前工作目录中的文件并将文件名传递给 `sed`。

```sh
      find . -type f -exec sed -i 's/{search_regex}/{replace_value}/g' {} + 

```

### 语法：

```sh
      sed [OPTION]... {script-only-if-no-other-script} [INPUT-FILE]... 

```

+   `OPTION` - 在原地、静默、跟随符号链接、行长度、null-data 等 sed 选项。

+   `{script-only-if-no-other-script}` - 如果可用，将脚本添加到命令中。

+   `INPUT-FILE` - 输入流，文件或来自管道的输入。

如果没有提供选项，则第一个非选项参数被视为要解释的 sed 脚本。所有剩余的参数都是输入文件名；如果没有指定输入文件，则读取标准输入。

GNU `sed` 主页：[`www.gnu.org/software/sed/`](http://www.gnu.org/software/sed/)

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-i[SUFFIX]` | --in-place[=SUFFIX] | 在原地编辑文件（如果提供后缀，则创建备份）。 |
| `-n` | --quiet, --silent | 抑制自动打印模式空间。 |
| `-e script` | --expression=script | 将脚本添加到要执行的命令中。 |
| `-f script-file` | --file=script-file | 将脚本文件的内容添加到要执行的命令中。 |
| `-l N` | --line-length=N | 为 `l` 命令指定所需的行换行长度。 |
| `-r` | --regexp-extended | 在脚本中使用扩展正则表达式。 |
| `-s` | --separate | 将文件视为单独的，而不是单个连续的长流。 |
| `-u` | --unbuffered | 从输入文件中加载最小量的数据，并更频繁地刷新输出缓冲区。 |
| `-z` | --null-data | 使用 NULL 字符分隔行。 |

### 在开始之前

最初可能看起来复杂且难以理解，但使用 sed 在文件中搜索和替换文本其实非常简单。

想要了解更多信息：[`www.gnu.org/software/sed/manual/sed.html`](https://www.gnu.org/software/sed/manual/sed.html)
