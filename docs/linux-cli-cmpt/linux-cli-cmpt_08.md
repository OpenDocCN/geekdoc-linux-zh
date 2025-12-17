# 搜索文件和文件名

> 原文：[`learnbyexample.github.io/cli-computing/searching-files-and-filenames.html`](https://learnbyexample.github.io/cli-computing/searching-files-and-filenames.html)

本章将展示如何根据文字字符串或正则表达式搜索文件内容。之后，你将学习如何根据文件名和其他属性（如大小、最后修改时间戳等）定位文件。

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) [example_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files) 目录包含本章使用的脚本。

## grep

引自 [wikipedia](https://en.wikipedia.org/wiki/Grep)：

> **`grep`** 是一个用于在纯文本数据集中搜索匹配正则表达式的行的命令行实用程序。其名称来自 `ed` 命令 `g/re/p`（**g**lobally search a **r**egular **e**xpression and **p**rint），具有相同的效果。

`grep` 命令有很多很多的功能，以至于我写了一本 [书](https://github.com/learnbyexample/learn_gnugrep_ripgrep) ，里面有数百个示例和练习。最常用的用法是使用正则表达式（regexp）从输入中过滤行。

### 常用选项

常用选项如下。示例将在后面的章节中讨论。

+   `--color=auto` 使用颜色突出显示匹配的部分、文件名、行号等

+   `-i` 忽略大小写

+   `-v` 仅打印非匹配行

+   `-n` 为输出行添加行号前缀

+   `-c` 仅显示输出行的计数

+   `-l` 仅打印匹配给定表达式的文件名

+   `-L` 打印不匹配模式的文件名

+   `-w` 仅匹配整个单词的模式

+   `-x` 仅匹配整个行的模式

+   `-F` 将模式解释为固定字符串（即不作为正则表达式）

+   `-o` 仅打印匹配的部分

+   `-A N` 打印匹配的行以及匹配行之后的 `N` 行

+   `-B N` 打印匹配的行以及匹配行之前的 `N` 行

+   `-C N` 打印匹配的行以及匹配行之前和之后的 `N` 行

+   `-m N` 打印最多 `N` 行匹配行

+   `-q` 无标准输出，如果找到匹配项则立即退出，在脚本中很有用

+   `-s` 抑制错误消息，在脚本中很有用

+   `-r` 递归搜索指定输入文件夹中的所有文件（默认搜索当前目录）

+   `-R` 类似于 `-r`，但也会跟随符号链接

+   `-h` 不要为匹配行添加文件名前缀（对于单个输入文件是默认行为）

+   `-H` 为匹配行添加文件名前缀（对于多个输入文件是默认行为）

### 文字搜索

以下示例都适合使用 `-F` 选项，因为这些示例不使用正则表达式。`grep` 在这种情况下足够智能，能够正确处理。

```sh
# lines containing 'an'
$ printf 'apple\nbanana\nmango\nfig\ntango\n' | grep 'an'
banana
mango
tango

# case insensitive matching
$ printf 'Cat\ncut\ncOnCaT\nfour cats\n' | grep -i 'cat'
Cat
cOnCaT
four cats

# match only whole words
$ printf 'par value\nheir apparent\ntar-par' | grep -w 'par'
par value
tar-par

# count empty lines
$ printf 'hi\n\nhello\n\n\n\nbye\n' | grep -cx ''
4

# print the matching line as well as two lines after
$ printf 'red\nblue\ngreen\nbrown\nyellow' | grep -A2 'blue'
blue
green
brown 
```

下面是一个示例，其中行号和匹配的部分以颜色突出显示：

![带有 --color 选项的示例](img/b636834e79fd5c1be8f83f7d8d2bc404.png)

### 正则表达式

默认情况下，`grep`将搜索模式视为基本正则表达式（BRE）。以下是与正则表达式相关的各种选项：

+   `-G`选项可以用来明确指定需要 BRE

+   `-E`选项将启用扩展正则表达式（ERE）

    +   在`GNU grep`中，基本正则表达式（BRE）和扩展正则表达式（ERE）的区别仅在于元字符的指定方式，功能上没有差异

+   `-F`选项将导致搜索模式被当作字面量处理

+   `-P`如果可用，此选项将启用 Perl 兼容正则表达式（PCRE）

以下参考内容是关于**扩展正则表达式**。

**锚点**

+   `^`将匹配限制在字符串的开始处

+   `$`将匹配限制在字符串的末尾

+   `\<`将匹配限制在单词的开始处

+   `\>`将匹配限制在单词的末尾

+   `\b`将匹配限制在单词的开始/结束处

+   `\B`匹配`\b`不匹配的地方

**点元字符和量词**

+   `.`匹配任何字符，包括换行符

+   `?`匹配 0 次或 1 次

+   `*`匹配 0 次或多次

+   `+`匹配 1 次或更多次

+   `{m,n}`匹配 m 到 n 次

+   `{m,}`至少匹配 m 次

+   `{,n}`匹配最多 n 次（包括 0 次）

+   `{n}`匹配恰好 n 次

**字符类**

+   `[set123]`匹配这些字符中的任意一个一次

+   `[^set123]`匹配除了这些字符之外的任意字符一次

+   `[3-7AM-X]`字符范围从`3`到`7`，`A`，另一个范围从`M`到`X`

+   `\w`类似于`[a-zA-Z0-9_]`，用于匹配单词字符

+   `\s`类似于`[ \t\n\r\f\v]`，用于匹配空白字符

+   `\W`匹配非单词字符

+   `\S`匹配非空白字符

+   `[[:digit:]]`类似于`[0-9]`

+   `[[:alnum:]_]`类似于`\w`

    +   有关完整列表，请参阅[grep 手册](https://www.gnu.org/software/grep/manual/grep.html#Character-Classes-and-Bracket-Expressions)

**交替和分组**

+   `pat1|pat2|pat3`匹配`pat1`或`pat2`或`pat3`

+   `()`分组模式，`a(b|c)d`等同于`abd|acd`

    +   也用作捕获组

+   `\N`反向引用，给出第 N 个捕获组匹配的部分

    +   `\1`反向引用第一个捕获组

    +   `\2`反向引用第二个捕获组，以此类推，直到`\9`

引用手册中关于 BRE 与 ERE 差异的说明：

> 在基本正则表达式中，元字符`?`、`+`、`{`、`|`、`(`和`)`失去了它们的特殊意义；相反，使用转义版本`\?`、`\+`、`\{`、`\|`、`\(`和`\)`。

### 正则表达式示例

```sh
# lines ending with 'ar'
$ printf 'spared no one\npar\nspar\ndare' | grep 'ar$'
par
spar

# extract 'part' or 'parrot' or 'parent' case insensitively
$ echo 'par apartment PARROT parent' | grep -ioE 'par(en|ro)?t'
part
PARROT
parent

# extract quoted text
$ echo 'I like "mango" and "guava"' | grep -oE '"[^"]+"'
"mango"
"guava"

# 8 character lines having the same 3 lowercase letters at the start and end
$ grep -xE '([a-z]{3})..\1' /usr/share/dict/words
mesdames
respires
restores
testates 
```

### 文件之间的行比较

`-f`和`-x`选项可以组合使用，以获取两个文件之间的公共行或使用`-v`时的差异。如果想要将搜索字符串当作字面量处理，请添加`-F`（回想一下，正则表达式是默认设置）。

```sh
# change to the 'scripts' directory and source the 'grep.sh' script
$ source grep.sh

# common lines between two files
$ grep -Fxf colors_1 colors_2
yellow

# lines present in colors_2 but not in colors_1
$ grep -Fvxf colors_1 colors_2
blue
black
dark green

# lines present in colors_1 but not in colors_2
$ grep -Fvxf colors_2 colors_1
teal
light blue
brown 
```

### Perl 兼容正则表达式

与 BRE/ERE 相比，PCRE 具有许多高级功能。以下是一些示例：

```sh
# numbers >= 100, uses possessive quantifiers
$ echo '0501 035 154 12 26 98234' | grep -oP '0*+\d{3,}'
0501
154
98234

# extract digits only if preceded by =
$ echo '100 apple=42, fig=314 red:255' | grep -oP '=\K\d+'
42
314

# all digits and optional hyphen combo from the start of the line
$ echo '123-87-593 42 fig 314-12-111' | grep -oP '\G\d+-?'
123-
87-
593

# all whole words except 'bat' and 'map'
$ echo 'car2 bat cod map combat' | grep -oP '\b(bat|map)\b(*SKIP)(*F)|\w+'
car2
cod
combat 
```

查看 `man pcrepattern` 或 [PCRE 在线手册](https://www.pcre.org/original/doc/html/pcrepattern.html) 以获取文档。

### 递归搜索

您可以使用 `-r` 选项在指定的目录中进行递归搜索。默认情况下，将搜索当前目录。如果想要跟随输入目录中的符号链接，请使用 `-R`。指定符号链接作为参数时不需要 `-R` 选项。

这里有一些基本示例。即使只匹配了一个文件，递归搜索也会像指定了 `-H` 选项一样工作。此外，默认情况下也包括隐藏文件。

```sh
# change to the 'scripts' directory and source the 'grep.sh' script
$ source grep.sh
$ ls -AF
backups/  colors_1  colors_2  .hidden  projects/

# recursively search in the 'backups' directory
$ grep -r 'clear' backups
backups/dot_files/.bash_aliases:alias c=clear
# add the -h option to prevent filename prefix in the output
$ grep -rh 'clear' backups
alias c=clear

# by default, the current directory is used for recursive search
$ grep -rl 'clear'
.hidden
backups/dot_files/.bash_aliases 
```

您可以使用 *包含/排除* 选项进一步修剪要搜索的文件。请注意，即使递归搜索未激活，这些选项也会生效。

| 选项 | 描述 |
| --- | --- |
| `--include=GLOB` | 仅搜索与 GLOB 匹配的文件 |
| `--exclude=GLOB` | 跳过与 GLOB 匹配的文件 |
| `--exclude-from=FILE` | 跳过与 FILE 中任何文件模式匹配的文件 |
| `--exclude-dir=GLOB` | 跳过与 GLOB 匹配的目录 |

```sh
# default recursive search
$ grep -r 'Hello'
projects/python/hello.py:print("Hello, Python!")
projects/shell/hello.sh:echo "Hello, Bash!"

# limit the search to only filenames ending with '.py'
$ grep -r --include='*.py' 'Hello'
projects/python/hello.py:print("Hello, Python!")

# in some cases you can just use shell globs instead recursive grep
$ shopt -s globstar
$ grep -H 'Hello' **/*.py
projects/python/hello.py:print("Hello, Python!") 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) [ripgrep](https://github.com/BurntSushi/ripgrep) 是 `GNU grep` 的推荐替代品，具有高度优化的正则表达式引擎、并行搜索、基于 `.gitignore` 忽略文件等功能。

### grep 和 xargs

您可以使用 shell `|` 操作符将一个命令的输出作为另一个命令的输入。假设一个命令给您一个文件名列表，并且您想将此列表作为输入 *参数* 传递给另一个命令，您会怎么做？一个解决方案是使用 `xargs` 命令。以下是一个基本示例（假设文件名不会与 shell 元字符冲突）：

```sh
# an example command producing a list of filenames
$ grep -rl 'clear'
.hidden
backups/dot_files/.bash_aliases

# same as: head -n1 .hidden backups/dot_files/.bash_aliases
$ grep -rl 'clear' | xargs head -n1
==> .hidden <==
ghost

==> backups/dot_files/.bash_aliases <==
alias p=pwd 
```

空格、换行符、分号等字符对 shell 来说是特殊的。因此，包含这些字符的文件名必须正确引用。或者，在适用的情况下，您可以使用由 ASCII NUL 字符分隔的文件名列表（因为文件名不能包含 NUL 字符）。您可以使用 `grep -Z` 来用 NUL 分隔输出，并使用 `xargs -0` 将输入视为 NUL 分隔。以下是一个示例：

```sh
# consider this command that generates a list of filenames
$ grep -rl 'blue'
.hidden
colors_1
colors_2
backups/color list.txt

# example to show issues due to filenames containing shell metacharacters
# 'backups/color list.txt' is treated as two different files
$ grep -rl 'blue' | xargs grep -l 'teal'
colors_2
grep: backups/color: No such file or directory
grep: list.txt: No such file or directory

# use 'grep -Z' + 'xargs -0' combo for a robust solution
# match files containing both 'blue' and 'teal'
$ grep -rlZ 'blue' | xargs -0 grep -l 'teal'
colors_1 
```

注意，传递给 `xargs` 的命令不接受自定义的别名和函数。所以，如果您将 `grep` 别名为 `grep --color=auto`，如果输出没有被着色，请不要感到惊讶。有关详细信息和工作方案，请参阅 [unix.stackexchange: have xargs use alias instead of binary](https://unix.stackexchange.com/q/141367/109046)。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 您可以使用 `xargs -r` 来避免在文件名列表没有任何非空白字符（即列表为空）时运行命令。
> 
> ```sh
> # there's no file containing 'violet'
> # so, xargs doesn't get any filename, but grep is still run
> $ grep -rlZ 'violet' | xargs -0 grep -L 'brown'
> (standard input)
> 
> # using the -r option avoids running the command in such cases
> $ grep -rlZ 'violet' | xargs -r0 grep -L 'brown' 
> ```
> 
> ![warning](img/b5314b4a0acf0f436c4bf59486793342.png) ![warning](img/b5314b4a0acf0f436c4bf59486793342.png) 不要使用 `xargs -P` 来组合并行运行的输出，因为您可能会得到一个混乱的结果。`[parallel](https://www.gnu.org/software/parallel/)` 命令将是一个更好的选择。有关更多详细信息，请参阅 [unix.stackexchange: xargs 与 parallel](https://unix.stackexchange.com/q/104778/109046)。另请参阅 [unix.stackexchange: 何时使用 xargs](https://unix.stackexchange.com/q/24954/109046)。

### 进一步阅读

+   我的电子书 [使用 GNU grep 和 ripgrep 进行 CLI 文本处理](https://github.com/learnbyexample/learn_gnugrep_ripgrep)

    +   参见我的博客文章 [GNU BRE/ERE 技巧表](https://learnbyexample.github.io/gnu-bre-ere-cheatsheet/)

+   [为什么 GNU grep 很快](https://lists.freebsd.org/pipermail/freebsd-current/2010-August/019310.html)

+   [unix.stackexchange: grep -r 与 find+grep](https://unix.stackexchange.com/q/131535/109046)

## find

`find` 命令具有综合功能，可以根据文件和目录的名称、大小、时间戳等过滤文件和目录。更重要的是，`find` 帮助您对这样的过滤文件执行操作。

### 文件名

默认情况下，当你使用不带任何选项或路径的 `find` 命令时，你将获得当前目录及其子目录中的所有条目（包括隐藏的条目）。要搜索特定路径，它们应立即在 `find` 之后提及，即在任何选项之前。

```sh
# change to the 'scripts' directory and source the 'find.sh' script
$ source find.sh
$ ls -F
backups/    hello_world.py*  ip.txt     report.log  todos/
errors.log  hi.sh*           projects/  scripts@

$ cd projects
# same as: find .
$ find
.
./.venv
./tictactoe
./tictactoe/game.py
./calculator
./calculator/calc.sh

$ cd ..
$ find todos
todos
todos/books.txt
todos/TRIP.txt
todos/wow.txt 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 注意，默认情况下不会跟随符号链接。在这种情况下，您可以使用 `-L` 选项。

要根据特定标准匹配文件名，您可以使用通配符或正则表达式。对于通配符，您可以使用 `-name` 选项或不区分大小写的版本 `-iname`。这些将仅匹配基本名称，因此如果您将 `/` 作为模式的一部分使用，您将收到警告。如果您需要在模式中包含 `/`，则可以使用 `-path` 和 `-ipath`。与 `grep` 不同，全局模式与整个基本名称匹配（因为全局模式中没有开始/结束锚点）。

```sh
# filenames ending with '.log'
# 'find .' indicates the current working directory (CWD) as the path to search
$ find . -name '*.log'
./report.log
./backups/aug.log
./backups/jan.log
./errors.log

# match filenames containing 'ip' case-insensitively
# note the use of '*' on both sides of 'ip' to match the whole filename
# . is optional when CWD is the only path to search
$ find -iname '*ip*'
./todos/TRIP.txt
./scripts
./ip.txt

# names containing 'k' within the 'backups' and 'todos' directories
$ find backups todos -name '*k*'
backups
backups/bookmarks.html
todos/books.txt 
```

您可以使用 `-not`（或 `!`）运算符来反转匹配条件：

```sh
# same as: find todos ! -name '*[A-Z]*'
$ find todos -not -name '*[A-Z]*'
todos
todos/books.txt
todos/wow.txt 
```

您可以使用 `-regex` 和 `-iregex`（不区分大小写）选项根据正则表达式匹配文件名。在这种情况下，模式将匹配整个路径，因此 `/` 可以使用，无需特殊选项。默认正则表达式风味是 `emacs`，您可以通过使用 `-regextype` 选项来更改它。

```sh
# filename containing only uppercase alphabets and file extension is '.txt'
# note the use of '.*/' to match the entire file path
$ find -regex '.*/[A-Z]+\.txt'
./todos/TRIP.txt

# here 'egrep' flavor is being used
# filename starting and ending with the same word character (case-insensitive)
# and file extension is '.txt'
$ find -regextype egrep -iregex '.*/(\w).*\1\.txt'
./todos/wow.txt 
```

### 文件类型

`-type` 选项有助于根据文件类型过滤文件，如常规文件、目录、符号链接等。

```sh
# regular files
$ find projects -type f
projects/tictactoe/game.py
projects/calculator/calc.sh

# regular files that are hidden as well
$ find -type f -name '.*'
./.hidden
./backups/dot_files/.bashrc
./backups/dot_files/.inputrc
./backups/dot_files/.vimrc

# directories
$ find projects -type d
projects
projects/.venv
projects/tictactoe
projects/calculator

# symbolic links
$ find -type l
./scripts 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 您可以使用 `,` 来分隔多个文件类型。例如，`-type f,l` 将匹配常规文件和符号链接。
> 
> ```sh
> $ find -type f,l -name '*ip*'
> ./scripts
> ./ip.txt 
> ```

### 深度

被搜索的路径被认为是深度 `0`，搜索路径内的文件位于深度 `1`，子目录内的文件位于深度 `2`，依此类推。请注意，这些全局选项应该在 `-type`、`-name` 等其他类型的选项之前指定。

`-maxdepth` 选项将搜索限制在指定的最大深度：

```sh
# non-hidden regular files only in the current directory
# sub-directories will not be checked
# -not -name '.*' can also be used instead of -name '[^.]*'
$ find -maxdepth 1 -type f -name '[^.]*'
./report.log
./hi.sh
./errors.log
./hello_world.py
./ip.txt 
```

`-mindepth` 选项指定最小深度：

```sh
# recall that path being searched is considered as depth 0
# and contents within the search path are at depth 1
$ find -mindepth 1 -maxdepth 1 -type d
./projects
./todos
./backups

$ find -mindepth 3 -type f
./projects/tictactoe/game.py
./projects/calculator/calc.sh
./backups/dot_files/.bashrc
./backups/dot_files/.inputrc
./backups/dot_files/.vimrc 
```

### 年龄

考虑以下文件属性：

+   `a` 访问

+   `c` 状态改变

+   `m` 修改

上述前缀需要与 `time`（基于 24 小时周期）或 `min`（基于分钟）选项结合使用。例如，`-mtime`（24 小时）选项检查最后修改的时间戳，而 `-amin`（分钟）检查最后访问的时间戳。这些选项接受一个数字（整数或分数）参数，该参数可以进一步由 `+` 或 `-` 符号前缀。以下是一些示例：

```sh
# modified less than 24 hours ago
$ find -maxdepth 1 -type f -mtime 0
./hello_world.py
./ip.txt

# accessed between 24 to 48 hours ago
$ find -maxdepth 1 -type f -atime 1
./ip.txt
# accessed within the last 24 hours
$ find -maxdepth 1 -type f -atime -1
./hello_world.py
# accessed within the last 48 hours
$ find -maxdepth 1 -type f -atime -2
./hello_world.py
./ip.txt

# modified more than 20 days back
$ find -maxdepth 1 -type f -mtime +20
./.hidden
./report.log
./errors.log 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) `-daystart` 限定符将时间仅从一天的开始计算。例如，`-daystart -mtime 1` 将检查昨天修改的文件。

### 大小

你可以使用 `-size` 选项根据文件大小进行筛选。默认情况下，数字参数将被视为 512 字节块。你可以使用后缀 `c` 来指定字节数。后缀 `k`（千字节）、`M`（兆字节）和 `G`（吉字节）是按照 1024 的幂来计算的。

```sh
# greater than 10 * 1024 bytes
$ find -type f -size +10k
./report.log
./errors.log

# greater than 9 bytes and less than 50 bytes
$ find -type f -size +9c -size -50c
./hi.sh
./hello_world.py
./ip.txt

# exactly 10 bytes
$ find -type f -size 10c
./ip.txt 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 你也可以使用 `-empty` 选项代替 `-size 0`。

### 对匹配的文件执行操作

`-exec` 选项可以帮助你将匹配的文件传递给另一个命令。你可以选择为每个文件执行一次命令（通过使用 `\;`）或者只为所有匹配的文件执行一次（通过使用 `+`）。然而，如果文件数量太多，`find` 将根据需要使用更多的命令调用。由于分号字符是一个 shell 元字符（你也可以通过转义来代替转义），所以它被转义了。

你需要使用 `{}` 来表示传递给正在执行的命令的文件参数。以下是一些示例：

```sh
# count the number of characters for each matching file
# wc is called separately for each matching file
$ find -type f -size +9k -exec wc -c {} \;
1234567 ./report.log
54321 ./errors.log

# here, both matching files are passed together to the wc command
$ find -type f -size +9k -exec wc -c {} +
1234567 ./report.log
  54321 ./errors.log
1288888 total 
```

如在管理文件和目录章节中提到的，`cp` 和 `mv` 命令的 `-t` 选项可以帮助你在指定源文件之前指定目标目录。以下是一个示例：

```sh
$ mkdir rc_files
$ find backups/dot_files -type f -exec cp -t rc_files {} +

$ find rc_files -type f
rc_files/.bashrc
rc_files/.inputrc
rc_files/.vimrc

$ rm -r rc_files 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 你可以使用 `-delete` 选项代替调用 `rm` 命令来删除匹配的文件。然而，它不能删除非空目录，并且还有其他需要注意的问题。请参阅手册以获取更多详细信息。

### 多个标准

文件名可以与多个标准匹配，如 `-name`、`-size`、`-mtime` 等。你可以在它们之间使用运算符，并在 `\(` 和 `\)` 内分组，以构建复杂的表达式。

+   `-a` 或 `-and` 或没有运算符意味着两个表达式都必须满足

    +   如果第一个表达式为假，则第二个表达式不会被评估

+   `-o` 或 `-or` 表示表达式中的任何一个都必须满足

    +   如果第一个表达式为真，则第二个表达式不会被评估

+   `-not` 取反表达式的结果

    +   您也可以使用 `!`，但这可能需要转义或引用，具体取决于 shell

```sh
# names containing both 'x' and 'ip' in any order (case-insensitive)
$ find -iname '*x*' -iname '*ip*'
./todos/TRIP.txt
./ip.txt

# names containing 'sc' or size greater than 10k
$ find -name '*sc*' -or -size +10k
./report.log
./scripts
./errors.log

# except filenames containing 'o' or 'r' or 'txt'
$ find -type f -not \( -name '*[or]*' -or -name '*txt*' \)
./projects/tictactoe/game.py
./projects/calculator/calc.sh
./.hidden
./hi.sh 
```

### Prune

当你想阻止 `find` 进入特定的目录时，`-prune` 选项很有帮助。默认情况下，`find` 会遍历所有文件，即使给定的条件会导致丢弃输出中的那些结果。因此，使用 `-prune` 不仅有助于加快处理速度，还可能在尝试访问排除路径中的文件时导致错误的情况下有所帮助。

```sh
# regular files ending with '.log'
$ find -type f -name '*.log'
./report.log
./backups/aug.log
./backups/jan.log
./errors.log

# exclude the 'backups' directory
# note the use of -path when '/' is needed in the pattern
$ find -type f -not -path './backups/*' -prune -name '*.log'
./report.log
./errors.log 
```

在处理基于 Git 的版本控制项目时，使用 `-not -path '*/.git/*' -prune` 可以很有用。

### find 和 xargs

与前面看到的 `grep -Z` 和 `xargs -0` 组合类似，您可以使用 `find -print0` 和 `xargs -0` 的组合。`-exec` 选项对于大多数用例来说已经足够了，但如果您需要出于性能原因进行并行执行，那么 `xargs -P`（或 [parallel](https://www.gnu.org/software/parallel/) 命令）可能很有用。

这里是一个将过滤后的文件传递给 `sed`（**s**tream **ed**itor，将在多用途文本处理工具章节中讨论）的例子：

```sh
$ find -name '*.log'
./report.log
./backups/aug.log
./backups/jan.log
./errors.log

# for the filtered files, replace all occurrences of 'apple' with 'fig'
# 'sed -i' will edit the files inplace, so no output on the terminal
$ find -name '*.log' -print0 | xargs -r0 -n2 -P2 sed -i 's/apple/fig/g' 
```

在上述例子中，`-P2` 用于允许 `xargs` 同时运行两个进程（默认为一个进程）。你可以使用 `-P0` 允许 `xargs` 尽可能多地启动进程。`-n2` 选项用于将传递给每个 `sed` 调用的文件参数数量限制为 `2`，否则 `xargs` 很可能传递尽可能多的参数，从而减少/抵消并行化的效果。请注意，上述说明中使用的 `-n` 和 `-P` 的值只是随机示例，你需要根据你的特定用例进行微调。

### 进一步阅读

+   [mywiki.wooledge: 使用 find](https://mywiki.wooledge.org/UsingFind)

+   [unix.stackexchange: find 和 tar 示例](https://unix.stackexchange.com/q/282762/109046)

+   [unix.stackexchange: 为什么在 find 的输出上循环是坏做法？](https://unix.stackexchange.com/q/321697/109046)

## locate

`locate` 是一个比 `find` 命令更快的按名称搜索文件的替代方案。它基于数据库，由 [cron](https://en.wikipedia.org/wiki/Cron) 任务更新。因此，除非更新数据库，否则新文件可能不会出现在结果中。如果您的发行版中可用（例如，在类似 Debian 的系统上使用 `sudo apt install mlocate`）并且您记得文件名的一部分，则此命令非常有用。如果您必须搜索整个文件系统，那么与 `find` 命令相比，`locate` 命令将花费很长时间。

这里有一些例子：

+   `locate 'power'` 打印包含 `power` 的文件名的路径

    +   隐式地，`locate`会将字符串改为`*power*`，因为指定的字符串中没有通配符字符

+   `locate -b '\power.log'` 打印路径匹配字符串`power.log`正好位于路径末尾

    +   `/home/learnbyexample/power.log` 匹配

    +   `/home/learnbyexample/lowpower.log'` 不会匹配，因为文件名开头有其他字符

    +   使用`\`防止搜索字符串被隐式替换为`*power.log*`

+   `locate -b '\proj_adder'` `-b`选项也很有用，可以只打印匹配的目录名，否则该文件夹下的所有文件也会被显示

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 参见[unix.stackexchange: find 和 locate 的优缺点](https://unix.stackexchange.com/q/60205/109046)。

## 练习

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 对于`grep`练习，使用[example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files)目录作为输入文件，除非另有说明。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 对于`find`练习，使用`find.sh`脚本，除非另有说明。

**1)** 显示来自输入文件`blocks.txt`、`ip.txt`和`uniform.txt`包含`an`的行。显示带有和没有文件名前缀的结果。

```sh
# ???
blocks.txt:banana
ip.txt:light orange
uniform.txt:mango

# ???
banana
light orange
mango 
```

**2)** 显示来自`sample.txt`输入文件的包含完整单词`he`的行。

```sh
# ???
14) He he he 
```

**3)** 匹配只包含`car`的完整行，不考虑大小写。匹配行应显示带有行号前缀。

```sh
$ printf 'car\nscared\ntar car par\nCar\n' | grep # ???
1:car
4:Car 
```

**4)** 显示`purchases.txt`中除包含`tea`的行之外的所有行。

```sh
# ???
coffee
washing powder
coffee
toothpaste
soap 
```

**5)** 显示`sample.txt`中包含`do`但不包含`it`的所有行。

```sh
# ???
13) Much ado about nothing 
```

**6)** 对于输入文件`sample.txt`，过滤包含`do`的行，并显示这样的匹配行之后的行。

```sh
# ???
 6) Just do-it
 7) Believe it
--
13) Much ado about nothing
14) He he he 
```

**7)** 对于输入文件`sample.txt`，过滤包含`are`或`he`作为完整单词的行以及这样的匹配行之前的行。查阅`info grep`或[在线手册](https://www.gnu.org/software/grep/manual/grep.html)，并使用适当的选项，以确保输出中匹配行的组之间没有分隔符。

```sh
# ???
 3) Hi there
 4) How are you
13) Much ado about nothing
14) He he he 
```

**8)** 提取所有包含或不包含文本的`()`对，前提是它们内部不包含`()`字符。

```sh
$ echo 'I got (12) apples' | grep # ???
(12)

$ echo '((2 +3)*5)=25 and (4.3/2*()' | grep # ???
(2 +3)
() 
```

**9)** 对于给定的输入，匹配所有以`den`开头或以`ly`结尾的行。

```sh
$ lines='reply\n1 dentist\n2 lonely\neden\nfly away\ndent\n'

$ printf '%b' "$lines" | grep # ???
reply
2 lonely
dent 
```

**10)** 提取以`s`开头并包含`e`和`t`（顺序不限）的单词。

```sh
$ words='sequoia subtle exhibit sets tests sit store_2'

$ echo "$words" | grep # ???
subtle
sets
store_2 
```

**11)** 提取所有首尾字符相同的完整单词。

```sh
$ echo 'oreo not a _oh_ pip RoaR took 22 Pop' | grep # ???
oreo
a
_oh_
pip
RoaR
22 
```

**12)** 匹配所有包含`*[5]`的输入行。

```sh
$ printf '4*5]\n(9-2)*[5]\n[5]*3\nr*[5\n' | grep # ???
(9-2)*[5] 
```

**13)** 匹配以`hand`开头并立即跟随着`s`或`y`或`le`或没有其他字符的完整行。

```sh
$ lines='handed\nhand\nhandy\nunhand\nhands\nhandle\nhandss\n'

$ printf '%b' "$lines" | grep # ???
hand
handy
hands
handle 
```

**14)** 输入行有三个或更多由逗号`,`分隔的字段。从第二个字段提取到倒数第二个字段。换句话说，提取除了第一个和最后一个字段之外的字段。

```sh
$ printf 'apple,fig,cherry\ncat,dog,bat\n' | grep # ???
fig
dog

$ echo 'dragon,42,unicorn,3.14,shapeshifter\n' | grep # ???
42,unicorn,3.14 
```

**15)** 递归搜索包含 `ello` 的文件。

```sh
# change to the 'scripts' directory and source the 'grep.sh' script
$ source grep.sh

# ???
projects/python/hello.py
projects/shell/hello.sh
colors_1
colors_2 
```

**16)** 递归搜索包含 `blue` 的文件，但不要在 `backups` 目录内搜索。

```sh
# change to the 'scripts' directory and source the 'grep.sh' script
$ source grep.sh

# ???
.hidden
colors_1
colors_2 
```

**17)** 递归搜索包含 `blue` 的文件，但文件同时包含 `teal` 时不搜索。

```sh
# change to the 'scripts' directory and source the 'grep.sh' script
$ source grep.sh

# ???
.hidden
colors_2
backups/color list.txt 
```

**18)** 查找 `backups` 目录内的所有常规文件。

```sh
# change to the 'scripts' directory and source the 'find.sh' script
$ source find.sh

# ???
backups/dot_files/.bashrc
backups/dot_files/.inputrc
backups/dot_files/.vimrc
backups/aug.log
backups/bookmarks.html
backups/jan.log 
```

**19)** 查找所有扩展名以 `p` 或 `s` 或 `v` 开头的常规文件。

```sh
# ???
./projects/tictactoe/game.py
./projects/calculator/calc.sh
./hi.sh
./backups/dot_files/.vimrc
./hello_world.py 
```

**20)** 查找所有文件名中不包含小写字母 `g` 到 `l` 的常规文件。

```sh
# ???
./todos/TRIP.txt
./todos/wow.txt 
```

**21)** 查找所有路径中至少有一个目录名以 `p` 或 `d` 开头的常规文件。

```sh
# ???
./projects/tictactoe/game.py
./projects/calculator/calc.sh
./backups/dot_files/.bashrc
./backups/dot_files/.inputrc
./backups/dot_files/.vimrc 
```

**22)** 查找所有名称包含 `b` 或 `d` 的目录。

```sh
# ???
./todos
./backups
./backups/dot_files 
```

**23)** 查找所有隐藏目录。

```sh
# ???
./projects/.venv 
```

**24)** 查找深度恰好为 `2` 的所有常规文件。

```sh
# ???
./todos/books.txt
./todos/TRIP.txt
./todos/wow.txt
./backups/aug.log
./backups/bookmarks.html
./backups/jan.log 
```

**25)** `find -mtime` 和 `find -atime` 之间的区别是什么？这些选项与哪个时间周期相关？

**26)** 查找所有空常规文件。

```sh
# ???
./projects/tictactoe/game.py
./projects/calculator/calc.sh
./todos/books.txt
./todos/TRIP.txt
./todos/wow.txt
./backups/dot_files/.bashrc
./backups/dot_files/.inputrc
./backups/dot_files/.vimrc
./backups/aug.log
./backups/bookmarks.html
./backups/jan.log 
```

**27)** 创建一个名为 `filtered_files` 的目录。然后，将所有大于 `1` 字节大小的常规文件复制到该目录，但文件名不以 `.log` 结尾。

```sh
# ???
$ ls -A filtered_files
hello_world.py  .hidden  hi.sh  ip.txt 
```

**28)** 查找所有隐藏文件，但不要查找它们是之前创建的 `filtered_files` 目录的一部分。

```sh
# ???
./.hidden
./backups/dot_files/.bashrc
./backups/dot_files/.inputrc
./backups/dot_files/.vimrc 
```

**29)** 删除之前创建的 `filtered_files` 目录。然后，阅读 `find` 手册，找出如何仅列出可执行文件的方法。

```sh
# ???
./hi.sh
./hello_world.py 
```

**30)** 至少列出一种将 `find` 输出通过管道传递到 `xargs` 命令而不是使用 `find -exec` 选项的使用场景。

**31)** `locate` 命令是如何比等效的 `find` 命令运行得更快？
