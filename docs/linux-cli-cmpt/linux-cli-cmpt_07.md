# 查看部分或整个文件内容

> 原文：[`learnbyexample.github.io/cli-computing/viewing-part-or-whole-file-contents.html`](https://learnbyexample.github.io/cli-computing/viewing-part-or-whole-file-contents.html)

在本章中，您将学习如何在终端内查看文件内容。如果内容太长，您可以一次查看一屏的内容，或者只获取输入的开始/结束部分。用于这些目的的命令还具有其他功能，其中一些将在本章中讨论。

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) [example_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files) 目录包含本章中使用的示例输入文件。

## 猫

`cat` 命令的名字来源于 `concatenate`（连接）。它主要用于将多个文件的内容合并到一个文件中，或者将其作为输入传递给另一个命令。

常用选项如下：

+   `-n` 在每行输入前添加行号和制表符

+   `-b` 与 `-n` 类似，但不为空行编号

+   `-s` 将连续的空行压缩为单个空行

+   `-v` 使用 [尖括号表示法](https://en.wikipedia.org/wiki/ASCII_control_characters#Control_code_chart) 来查看特殊字符，如 NUL

+   `-e` 查看特殊字符并标记行尾

+   `-A` 包括 `-e` 并有助于识别制表符

这里有一些示例来展示 `cat` 的主要功能。可以提供一个或多个文件作为参数。

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) 如前所述，[example_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files) 目录包含本章中使用的示例输入文件。您需要进入 `example_files/text_files` 目录以跟随本章中展示的示例。

```sh
# view contents of a single file
$ cat greeting.txt
Hi there
Have a nice day

# another example
$ cat fruits.txt
banana
papaya
mango

# concatenate multiple files
$ cat greeting.txt fruits.txt
Hi there
Have a nice day
banana
papaya
mango 
```

要保存连接输出的结果，请使用重定向：

```sh
$ cat greeting.txt fruits.txt > op.txt

$ cat op.txt
Hi there
Have a nice day
banana
papaya
mango 
```

您可以使用 `-` 作为文件参数来表示 `stdin` 数据。如果没有提供文件参数，`cat` 将会读取 `stdin` 数据（如果存在）或等待交互式输入。请注意，`-` 也被许多其他命令支持，用来表示 `stdin` 数据。

```sh
# concatenate contents of 'greeting.txt' and 'stdin' data
$ echo 'apple banana cherry' | cat greeting.txt -
Hi there
Have a nice day
apple banana cherry 
```

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 `cat` 来查看文件内容、连接文件等是很好用的。但是，在不必要的时候使用 `cat` 是一个坏习惯，您应该避免。有关更多详情，请参阅 [wikipedia: UUOC](https://en.wikipedia.org/wiki/Cat_(Unix)#Useless_use_of_cat) 和 [Useless Use of Cat Award](https://porkmail.org/era/unix/award.html)。

`cat` 还可以帮助您使用 [尖括号表示法](https://en.wikipedia.org/wiki/ASCII_control_characters#Control_code_chart) 来识别特殊字符：

```sh
# example for backspace and carriage return characters
$ printf 'car\bd\nbike\rp\n'
cad
pike
$ printf 'car\bd\nbike\rp\n' | cat -v
car^Hd
bike^Mp

# example with tab characters and end-of-line marker
$ printf '1 2\t3\f4\v5   \n' | cat -A
1 2^I3^L4^K5   $ 
```

## tac

您可以使用 `tac` 来连接文件，但输出将会以相反的顺序（逐行）打印。如果您传递多个输入文件，每个文件的内容将会单独反转。以下是一些示例：

```sh
$ printf 'apple\nbanana\ncherry\n' | tac
cherry
banana
apple

# won't be same as: cat greeting.txt fruits.txt | tac
$ tac greeting.txt fruits.txt
Have a nice day
Hi there
mango
papaya
banana 
```

> ![警告](img/b5314b4a0acf0f436c4bf59486793342.png) 如果输入的最后一行不以换行符结束，则输出也不会有那个换行符。
> 
> ```sh
> $ printf 'apple\nbanana\ncherry' | tac
> cherrybanana
> apple 
> ```

## less

`cat`命令不适合在终端中查看大文件的文件内容。`less`命令自动将内容调整到终端的大小，允许滚动，并具有有效的查看功能。通常，`man`命令使用`less`作为`pager`来显示文档。导航选项与 Vim 文本编辑器类似。

常用命令如下。您可以按`h`键获取内置帮助。

+   使用`↑`和`↓`箭头键向上和向下移动一行

    +   您也可以使用`k`和`j`键（与 Vim 文本编辑器中使用的键相同）

+   使用`f`和`b`键向前和向后移动一屏的内容

    +   `Space`键也可以向前移动一屏

+   鼠标滚轮上下移动几行

+   使用`g`或`Home`键跳转到文件开头

+   使用`G`或`End`键跳转到文件末尾

+   在`/pattern`后按`Enter`键，在正向方向搜索给定的模式

    +   模式指的是正则表达式，并取决于您系统中的正则表达式库

    +   在我的系统上，正则表达式的风味是扩展正则表达式（ERE）

    +   查看`man re_format`获取更多详细信息

+   在`?pattern`后按`Enter`键，在反向方向搜索给定的模式

+   使用`n`键跳转到下一个匹配项

+   使用`N`键跳转到上一个匹配项

+   `q`退出

例如，使用`less /usr/share/dict/words`打开字典文件并练习上述命令。如果您的`pager`设置为`less`用于手册页，您也可以尝试类似`man ls`的命令进行练习。

与`cat`命令类似，您可以使用`-s`选项压缩连续的空白行。但与`cat -n`不同，您需要使用`less -N`来添加行号前缀。小写的`-n`选项将关闭编号。

**进一步阅读**

+   `less`命令是`more`命令的[改进版本](https://unix.stackexchange.com/q/604/109046)

+   [unix.stackexchange: most, more 和 less 的区别](https://unix.stackexchange.com/q/81129/109046)

+   我的[Vim 参考指南](https://github.com/learnbyexample/vim_reference)电子书

## tail

默认情况下，`tail`显示输入文件的最后 10 行。如果输入文件中的行数少于 10 行，则只显示这些行。您可以使用`-n`选项更改显示的行数。通过使用`tail -n +N`，您可以获取从第`N`行开始的全部行。

下面是一个用于说明目的的示例文件：

```sh
$ cat sample.txt 
 1) Hello World
 2) 
 3) Hi there
 4) How are you
 5) 
 6) Just do-it
 7) Believe it
 8) 
 9) banana
10) papaya
11) mango
12) 
13) Much ado about nothing
14) He he he
15) Adios amigo 
```

下面是一些使用`-n`选项的示例：

```sh
# last two lines (input has 15 lines)
$ tail -n2 sample.txt
14) He he he
15) Adios amigo

# all lines starting from the 11th line
# space between -n and +N is optional
$ tail -n +11 sample.txt
11) mango
12) 
13) Much ado about nothing
14) He he he
15) Adios amigo 
```

如果您传递多个输入文件，每个文件将单独处理。默认情况下，输出将带有文件标题和空行分隔符，您可以使用`-q`（安静）选项覆盖。

```sh
$ tail -n2 fruits.txt sample.txt 
==> fruits.txt <==
papaya
banana

==> sample.txt <==
14) He he he
15) Adios amigo 
```

`-c`选项与`-n`选项类似，但使用字节而不是行：

```sh
# last three bytes
# note that the input doesn't end with a newline character
$ printf 'apple pie' | tail -c3
pie

# starting from the fifth byte
$ printf 'car\njeep\nbus\n' | tail -c +5
jeep
bus 
```

**进一步阅读**

+   [wikipedia: 使用 tail -f 和 -F 选项进行文件监控](https://en.wikipedia.org/wiki/Tail_(Unix)#File_monitoring)

    +   [toolong](https://github.com/Textualize/toolong) — 终端应用程序，用于查看、尾部、合并和搜索日志文件

+   [unix.stackexchange: tail -f 选项是如何工作的？](https://unix.stackexchange.com/q/18760/109046)

+   [如何处理输出缓冲？](https://mywiki.wooledge.org/BashFAQ/009)

## head

默认情况下，`head` 会显示输入文件的前 10 行。如果输入行少于 10 行，则只显示这些行。你可以使用 `-n` 选项来更改显示的行数。通过使用 `head -n -N`，你可以得到除了最后 `N` 行之外的所有输入行。

```sh
# first three lines
$ head -n3 sample.txt
 1) Hello World
 2) 
 3) Hi there

# except the last 11 lines
$ head -n -11 sample.txt
 1) Hello World
 2) 
 3) Hi there
 4) How are you 
```

你可以通过组合 `head` 和 `tail` 命令来选择行范围。

```sh
# 9th to 11th lines
# same as: tail -n +9 sample.txt | head -n3
$ head -n11 sample.txt | tail -n +9
 9) banana
10) papaya
11) mango 
```

如果你传递多个输入文件，每个文件都会单独处理。默认情况下，输出会以文件名标题和空行分隔符格式化，你可以使用 `-q`（安静）选项来覆盖它。

```sh
$ printf '1\n2\n' | head -n1 greeting.txt -
==> greeting.txt <==
Hi there

==> standard input <==
1 
```

`-c` 选项与 `-n` 选项类似，但使用字节而不是行：

```sh
# first three bytes
$ printf 'apple pie' | head -c3
app

# excluding the last four bytes
$ printf 'car\njeep\nbus\n' | head -c -4
car
jeep 
```

## 练习

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录中的输入文件进行以下练习。

**1)** 你会使用哪些选项来得到以下显示的输出？

```sh
$ printf '\n\n\ndragon\n\n\nunicorn\n\n\n' | cat # ???

     1  dragon

     2  unicorn 
```

**2)** 向 `cat` 命令传递适当的参数以得到以下显示的输出。

```sh
$ cat greeting.txt
Hi there
Have a nice day

$ echo '42 apples and 100 bananas' | cat # ???
42 apples and 100 bananas
Hi there
Have a nice day 
```

**3)** 以下两个命令会产生相同的输出吗？如果不相同，原因是什么？

```sh
$ cat fruits.txt ip.txt | tac

$ tac fruits.txt ip.txt 
```

**4)** 阅读`tac`命令的手册，并使用适当的选项和参数来得到以下显示的输出。

```sh
$ cat blocks.txt
%=%=
apple
banana
%=%=
brown
green

# ???
%=%=
brown
green
%=%=
apple
banana 
```

**5)** `less -n` 和 `less -N` 选项之间的区别是什么？`cat -n` 和 `less -n` 有类似的功能吗？

**6)** 你会使用哪个命令在现有的 `less` 会话中打开另一个文件？以及你会使用哪些命令在前后文件之间导航？

**7)** 使用适当的命令和 shell 功能来得到以下显示的输出。

```sh
$ printf 'carpet\njeep\nbus\n'
carpet
jeep
bus

# use the above 'printf' command for input data
$ c=# ???
$ echo "$c"
car 
```

**8)** 如何显示除了第一行之外的所有输入行？

```sh
$ printf 'apple\nfig\ncarpet\njeep\nbus\n' | # ???
fig
carpet
jeep
bus 
```

**9)** 你会使用哪些命令来得到以下显示的输出？

```sh
$ cat fruits.txt
banana
papaya
mango
$ cat blocks.txt
%=%=
apple
banana
%=%=
brown
green

# ???
banana
papaya
%=%=
apple 
```

**10)** 使用 `head` 和 `tail` 命令的组合来获取给定输入的第 11 到 14 个字符。

```sh
$ printf 'apple\nfig\ncarpet\njeep\nbus\n' | # ???
carp 
```

**11)** 从输入文件 `table.txt` 和 `fruits.txt` 中提取前六个字节。

```sh
# ???
brown banana 
```

**12)** 从输入文件 `fruits.txt` 和 `table.txt` 中提取最后六个字节。

```sh
# ???
mango
 3.14 
```
