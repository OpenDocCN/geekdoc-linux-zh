# 比较文件

> 原文：[`learnbyexample.github.io/cli-computing/comparing-files.html`](https://learnbyexample.github.io/cli-computing/comparing-files.html)

在本章中，你将学习如何查找和报告两个文件内容之间的差异。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) [example_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files) 目录包含本章中使用的示例输入文件。

## cmp

`cmp` 命令用于比较文本和二进制文件。如果两个输入文件内容相同，则不显示输出并退出状态为 `0`。如果有差异，则打印第一个差异的详细信息，如行号和字节位置，退出状态将为 `1`。

```sh
$ mkdir practice_cmp
$ cd practice_cmp
$ echo 'hello' > x1.txt
$ cp x{1,2}.txt
$ echo 'hello.' > x3.txt

# files with the same content
$ cmp x1.txt x2.txt
$ echo $?
0

# files with differences
$ cmp x1.txt x3.txt
x1.txt x3.txt differ: byte 6, line 1
$ echo $?
1 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 `-s` 选项来抑制输出，当你只需要退出状态时。`-i` 选项将允许你跳过输入的初始字节。 

## diff

有助于查找文本文件之间的差异。所有差异都会打印出来，这可能不适合长文件。

### 常用选项

常用选项如下。示例将在后续章节中讨论。

+   `-i` 忽略大小写

+   `-w` 忽略空白字符

+   `-b` 忽略空白数量的变化

+   `-B` 忽略空白行

+   `-E` 忽略由于制表符扩展引起的变化

+   `-z` 忽略行尾的空白字符

+   `-y` 两列输出

+   `-r` 递归地比较两个指定目录之间的文件

+   `-s` 当两个文件相同时代码显示消息

+   `-q` 报告文件是否不同，而不是差异的详细信息

### 默认 diff

默认情况下，`diff` 输出显示以 `<` 为前缀的第一输入文件的行，以及以 `>` 为前缀的第二文件的行。使用 `---` 作为分组分隔符。每个差异都由一个表示差异的命令前缀，这些命令被像 `patch` 这样的工具所理解。

```sh
# change to the 'example_files/text_files' directory
# side-by-side view of sample input files
$ paste f1.txt f2.txt
1       1
2       hello
3       3
world   4

$ diff f1.txt f2.txt
2c2
< 2
---
> hello
4c4
< world
---
> 4

$ diff <(seq 4) <(seq 5)
4a5
> 5 
```

### 忽略空白字符

在比较过程中有几种选项可以忽略特定的空白字符。以下是一些示例：

```sh
# ignore changes in the amount of whitespace
$ diff -b <(echo 'good day') <(echo 'good    day')
$ echo $?
0

# ignore all whitespaces
$ diff -w <(echo 'hi    there ') <(echo ' hi there')
$ echo $?
0
$ diff -w <(echo 'hi    there ') <(echo 'hithere')
$ echo $?
0 
```

### 并排输出

`-y` 选项方便查看并排的差异。默认情况下，所有输入行都会出现在输出中，行宽为 130 个打印列。当处理短输入行时，你可以使用 `-W` 选项来更改宽度。`--suppress-common-lines` 帮助你只关注差异。

```sh
$ diff -y f1.txt f2.txt
1                                                               1
2                                                             | hello
3                                                               3
world                                                         | 4

$ diff -W 60 --suppress-common-lines -y f1.txt f2.txt
2                            |  hello
world                        |  4 
```

### 进一步阅读

+   `gvimdiff` 使用 GVim 编辑两个、三个或四个版本的文件并显示差异

+   [GUI diff 和合并工具](http://askubuntu.com/questions/2946/what-are-some-good-gui-diff-and-merge-applications-available-for-ubuntu)

+   [difftastic](https://github.com/Wilfred/difftastic) — 理解语法的结构化差异工具

+   [icdiff](https://github.com/jeffkaufman/icdiff) — 改进的彩色差异

## 练习

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录中的输入文件进行以下练习。

**1)** 如果你只需要反映给定输入是否相同的退出状态，你会使用哪个 `cmp` 选项？

**2)** 你会使用哪个 `cmp` 选项来跳过比较目的的初始字节？以下示例需要你跳过前两个字节。

```sh
$ echo '1) apple' > x1.txt
$ echo '2\. apple' > x2.txt
$ cmp x1.txt x2.txt
x1.txt x2.txt differ: byte 1, line 1

$ cmp # ???
$ echo $?
0

$ rm x[12].txt 
```

**3)** `diff -d` 选项的作用是什么？

**4)** 哪个选项可以帮助你使用 `diff` 获得彩色输出？

**5)** 使用适当的选项以获得以下所示所需的输出。

```sh
# instead of this output
$ diff -W 40 --suppress-common-lines -y f1.txt f2.txt
2                  |    hello
world              |    4

# get this output
$ diff # ???
1                  (
2                  |    hello
3                  (
world              |    4 
```

**6)** 使用适当的选项以获得以下所示所需的输出。

```sh
$ echo 'hello' > d1.txt
$ echo 'Hello' > d2.txt

# instead of this output
$ diff d1.txt d2.txt
1c1
< hello
---
> Hello

# get this output
$ diff # ???
Files d1.txt and d2.txt are identical

$ rm d[12].txt 
```
