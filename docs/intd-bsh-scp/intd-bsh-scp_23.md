# Bash 中的重定向

Linux 系统管理员必须对 Bash 中的管道和重定向有很好的了解。它是系统的一个基本组成部分，并且在 Linux 系统管理领域经常很有帮助。

当你运行像 `ls`、`cat` 等命令时，你会在终端上得到一些输出。如果你输入了错误的命令，传递了错误的标志或命令行参数，你会在终端上得到错误输出。在这两种情况下，你都会得到一些文本。这可能看起来像是“仅仅是文本”，但系统会以不同的方式处理这些文本。这个标识符被称为文件描述符（fd）。

在 Linux 中，有 3 个文件描述符，**STDIN**（0）；**STDOUT**（1）和**STDERR**（2）。

+   **STDIN**（fd: 0）：管理终端中的输入。

+   **STDOUT**（fd: 1）：管理终端中的输出文本。

+   **STDERR**（fd: 2）：管理终端中的错误文本。

# 管道和重定向的区别

**管道**和**重定向**都会重定向正在执行的进程的流（文件描述符）。主要区别在于**重定向**处理**文件流**，将输出流发送到文件或将给定文件的 内容发送到进程的输入流。

另一方面，管道通过将第一个命令的输出流发送到第二个命令的输入流来连接两个命令，而不指定任何重定向。

# Bash 中的重定向

## 标准输入（STDIN）

当你为请求输入文本的命令输入一些输入文本时，你实际上是将文本输入到 **STDIN** 文件描述符中。运行没有任何命令行参数的 `cat` 命令。它可能看起来像进程已经暂停了，但实际上是 `cat` 正在请求 **STDIN**。`cat` 是一个简单的程序，它会打印传递给 **STDIN** 的文本。然而，你可以通过将输入重定向到接受 **STDIN** 的命令来扩展用例。

使用 `cat` 的示例：

```sh
      cat << EOF
Hello World!
How are you?
EOF 

```

这个命令将简单地将在终端屏幕上打印提供的文本：

```sh
      Hello World!
How are you? 

```

同样，你也可以用其他通过 STDIN 获取输入的命令来做这件事。比如，`wc`：

```sh
      wc -l << EOF
Hello World!
How are you?
EOF 

```

`wc` 的 `-l` 标志计算行数。这个 bash 代码块将打印行数到终端屏幕：

```sh
      2 

```

## 标准输出（STDOUT）

你终端屏幕上的正常非错误文本是通过 **STDOUT** 文件描述符打印的。命令的 **STDOUT** 可以重定向到文件，这样命令的输出就会写入文件而不是打印在终端屏幕上。这可以通过 `>` 和 `>>` 运算符简单地完成。

示例：

```sh
      echo "Hello World!" > file.txt 

```

上述命令不会在终端屏幕上打印“Hello World”，而是会创建一个名为 `file.txt` 的文件，并将“Hello World”字符串写入其中。这可以通过在 `file.txt` 文件上运行 `cat` 命令来验证。

```sh
      cat file.txt 

```

然而，每次你将任何命令的 **STDOUT** 多次重定向到同一个文件时，它都会删除文件中的现有内容以写入新的内容。

示例：

```sh
      echo "Hello World!" > file.txt
echo "How are you?" > file.txt 

```

在对 `file.txt` 文件运行 `cat` 命令时：

```sh
      cat file.txt 

```

你只会得到“你好吗？”字符串打印出来。

```sh
      How are you? 

```

这是因为 "Hello World" 字符串已被覆盖。这种行为可以通过使用 `>>` 操作符来避免。

上述示例可以写成如下形式：

```sh
      echo "Hello World!" > file.txt
echo "How are you?" >> file.txt 

```

在对 `file.txt` 文件运行 `cat` 命令时，你会得到期望的结果。

```sh
      Hello World!
How are you? 

```

或者，**STDOUT** 的重定向操作符也可以写成 `1>`。例如，

```sh
      echo "Hello World!" 1> file.txt 

```

## STDERR (标准错误)

终端屏幕上的错误文本是通过命令的 **STDERR** 打印的。例如：

```sh
      ls --hello 

```

会给出错误信息。这个错误信息是命令的 **STDERR**。

**STDERR** 可以使用 `2>` 操作符进行重定向。

```sh
      ls --hello 2> error.txt 

```

此命令将错误信息重定向到 `error.txt` 文件并写入其中。这可以通过在 `error.txt` 文件上运行 `cat` 命令来验证。

你也可以像使用 `>>` 操作符来处理 **STDOUT** 一样，使用 `2>>` 操作符来处理 **STDERR**。

在 Bash 脚本中，错误信息有时可能是不希望的。你可以选择通过将错误信息重定向到 `/dev/null` 文件来忽略它们。`/dev/null` 是一个伪设备，它接收文本然后立即丢弃它。

上述示例可以写成如下形式以完全忽略错误文本：

```sh
      ls --hello 2> /dev/null 

```

当然，你可以将同一个命令或脚本的 **STDOUT** 和 **STDERR** 都重定向到同一个文件。

```sh
      ./install_package.sh > output.txt 2> error.txt 

```

它们都可以重定向到同一个文件。

```sh
      ./install_package.sh > file.txt 2> file.txt 

```

还有更简单的方法来做这件事。

```sh
      ./install_package.sh > file.txt 2>&1 

```

# 管道操作

到目前为止，我们已经看到了如何将 **STDOUT**、**STDIN** 和 **STDOUT** 重定向到和从文件。要连接程序 *(命令)* 的输出作为另一个程序 *(命令)* 的输入，你可以使用垂直线 `|`。

示例：

```sh
      ls | grep ".txt" 

```

此命令将列出当前目录中的文件，并将输出传递给 *`grep`* 命令，然后过滤输出，只显示包含字符串 ".txt" 的文件。

语法：

```sh
      [time [-p]] [!] command1 [ | or |& command2 ] … 

```

你也可以通过将它们管道连接起来来构建任意命令链，以实现强大的结果。

此示例创建了一个列表，列出给定目录中每个用户拥有的文件以及他们拥有的文件和目录数量：

```sh
      ls -l /projects/bash_scripts | tail -n +2 | sed 's/\s\s*/ /g' | cut -d ' ' -f 3 | sort | uniq -c 

```

输出：

```sh
      8 anne
34 harry
37 tina
18 ryan 

```

# HereDocument

符号 `<<` 可以用来创建一个临时文件 [heredoc] 并在命令行中从该文件重定向。

```sh
      COMMAND << EOF
	ContentOfDocument
	...
	...
EOF 

```

注意，这里的 `EOF` 代表 heredoc 的分隔符（文件结束）。实际上，我们可以用任何字母数字词来代替它，以表示文件的开始和结束。例如，这是一个有效的 heredoc：

```sh
      cat << randomword1
	This script will print these lines on the terminal.
	Note that cat can read from standard input. Using this heredoc, we can
	create a temporary file with these lines as it's content and pipe that
	into cat.
randomword1 

```

实际上，这会看起来像是 heredoc 的内容被管道输入到命令中。如果需要将多行管道输入到程序中，这可以使脚本非常干净。

此外，我们还可以附加更多的管道，如下所示：

```sh
      cat << randomword1 | wc
	This script will print these lines on the terminal.
	Note that cat can read from standard input. Using this heredoc, we can
	create a temporary file with these lines as it's content and pipe that
	into cat.
randomword1 

```

此外，在 heredocs 中也可以使用预定义的变量。

# HereString

Herestrings 与 heredocs 非常相似，但使用 `<<<`。这些用于需要管道到某个程序的单一行字符串。与 heredocs 相比，这看起来更干净，因为我们不需要指定分隔符。

```sh
      wc <<<"this is an easy way of passing strings to the stdin of a program (here wc)" 

```

就像 heredocs 一样，herestrings 也可以包含变量。

## 摘要

| **操作符** | **描述** |
| :-- | :-- |
| `>` | `将输出保存到文件` |
| `>>` | 将输出追加到文件 |
| `<` | 从文件读取输入 |
| `2>` | 重定向错误信息 |
| `&#124;` | 将一个程序的输出作为另一个程序的输入 |
| `<<` | 清洁地将多行管道输入到程序 |
| `<<<` | 清洁地将单行管道输入到程序 |
