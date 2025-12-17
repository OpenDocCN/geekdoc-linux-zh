# Shell 自定义

> 原文：[`learnbyexample.github.io/cli-computing/shell-customization.html`](https://learnbyexample.github.io/cli-computing/shell-customization.html)

本章将讨论一些你可以用来自定义命令行环境的 `bash` 功能。

## 环境变量

来自 [wikipedia: 环境变量](https://en.wikipedia.org/wiki/Environment_variable):

> 环境变量是动态命名的值，可以影响计算机上运行进程的行为方式。它们是进程运行的环境的一部分。例如，一个正在运行的进程可以查询 TEMP 环境变量的值以发现存储临时文件的一个合适位置，或者使用 HOME 或 USERPROFILE 变量来找到运行进程的用户拥有的目录结构。

查看 [bash 手册: Shell 变量](https://www.gnu.org/software/bash/manual/bash.html#Shell-Variables) 以获取 `bash` 变量的完整列表。其中一些将在本章后面讨论，例如 `HISTCONTROL`。

+   `HOME` 当前用户的家目录；`cd` 内置命令的默认值。此变量的值也用于波浪线展开

+   `PS1` 主要提示字符串。默认值是 `\s-\v\$`

+   `PS2` 二级提示字符串。默认值是 `>`

+   `PATH` 壳层查找命令的目录列表，由冒号分隔。`PATH` 值中的零长度（空）目录名表示当前目录。空目录名可能以两个相邻的冒号出现，或以初始或结尾的冒号出现

+   `PWD` 由 `cd` 内置命令设置当前工作目录

+   `OLDPWD` 由 `cd` 内置命令设置的先前工作目录

+   `SHELL` 此环境变量展开为壳层的完整路径名

你可以使用 `printenv` 命令显示所有环境变量的名称和值。提供参数将只显示那些变量的值。

```sh
$ printenv SHELL PWD HOME
/bin/bash
/home/learnbyexample/cli-computing
/home/learnbyexample 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) ![warning](img/b5314b4a0acf0f436c4bf59486793342.png) 建议使用小写字母为用户定义的变量命名，以避免与环境变量发生潜在冲突。你可能已经注意到，我在 Shell 脚本 章节中只使用了小写名称。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 参见 [unix.stackexchange: 如何正确地将路径添加到 PATH？](https://unix.stackexchange.com/q/26047/109046)。

## 别名和函数

要创建别名，请使用名称合适的 `alias` 命令。如果不提供任何参数，它将列出所有当前定义的别名。如果您想了解现有别名的作用，请提供一个或多个名称作为参数。要实际创建别名，给出一个名称，然后跟 `=` 和要别名的命令。`=` 操作符周围不应有空格。使用 `type name` 检查该名称是否已被某些命令占用。以下是一些示例：

```sh
# mapping 'p' to the 'pwd' command
$ type p
bash: type: p: not found
$ alias p='pwd'
$ p
/home/learnbyexample/cli-computing

# adding '--color=auto' to 'ls' invocations
$ type -a ls
ls is /bin/ls
$ alias ls='ls --color=auto'
$ type -a ls
ls is aliased to 'ls --color=auto'
ls is /bin/ls 
```

这是检查上述别名如何工作的方法：

```sh
$ alias p ls
alias p='pwd'
alias ls='ls --color=auto' 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 如上图所示，别名相对于 PATH 中的命令具有更高的优先级。如果您想避免别名并使用原始命令，可以使用 `\` 前缀（例如 `\ls`）。您也可以使用 `command ls` 而不是转义字符。

如果您需要向自定义命令传递参数，请使用函数（或编写 shell 脚本）。以下是一个示例函数：

```sh
# prefix current path to the given arguments
$ ap() { for f in "$@"; do echo "$PWD/$f"; done; }

$ p
/home/learnbyexample
$ ap ip.txt mountain.jpg
/home/learnbyexample/ip.txt
/home/learnbyexample/mountain.jpg 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 上文中创建的别名和函数仅对特定的 shell 会话有效。要自动加载这些快捷方式，您需要将它们添加到特殊文件中。有关详细信息，请参阅下一节。

您可以使用 `unalias` 命令来删除别名。对于函数，请使用 `unset -f` 命令。

```sh
$ unalias p

$ unset -f ap

$ type p ap
bash: type: p: not found
bash: type: ap: not found 
```

## 配置文件

您可以将自定义设置添加到特殊的配置文件中，以便在启动交互式 shell 会话时自动加载这些设置。

### .bashrc

来自 [bash 手册：启动文件](https://www.gnu.org/software/bash/manual/bash.html#Bash-Startup-Files)：

> 当启动的不是登录 shell 的交互式 shell 时，Bash 会读取并执行 `~/.bashrc` 文件中的命令，如果该文件存在的话。

您可能有一个由您安装的 Linux 发行版提供的 `~/.bashrc` 文件，其中包含启用 `bash` 可编程完成功能、别名等有用的设置。我保留发行版提供的设置不变，除非它们与我想定制的别名和 shell 选项相关。

我使用的 `shopt` 自定义设置如下所示。`shopt` 在 Shell 功能 章节中简要讨论过。有关更多详细信息，请参阅 [bash 手册：Shopt 内置函数](https://www.gnu.org/software/bash/manual/bash.html#The-Shopt-Builtin)。

```sh
# append to history file instead of overwriting
shopt -s histappend

# extended wildcard functionality
shopt -s extglob

# helps to recursively match files within a specified path
shopt -s globstar 
```

我更喜欢一个简单的提示符 `PS1='$ '` 而不是花哨的颜色。有关自定义选项，请参阅 [bash 手册：控制提示符](https://www.gnu.org/software/bash/manual/bash.html#Controlling-the-Prompt)。还可以查看 [starship](https://starship.rs/)，这是一个最小化、速度极快且无限可定制的任何 shell 的提示符。

下面显示了某些历史自定义设置。有关更多详细信息，请参阅 [bash 手册：历史功能](https://www.gnu.org/software/bash/manual/bash.html#Bash-History-Facilities)。还可以参阅 [unix.stackexchange：会话间的常见历史](https://unix.stackexchange.com/q/18212/109046)。

```sh
# ignorespace prevents lines starting with space from being saved in history
# erasedups deletes previous history entries matching the current one
HISTCONTROL=ignorespace:erasedups

# maximum number of history lines in the current shell session
# older entries will be overwritten if the size is exceeded
# use a negative number for unlimited size
HISTSIZE=2000

# maximum number of lines in the history file
HISTFILESIZE=2000 
```

对于别名和函数，我使用一个名为 `~/.bash_aliases` 的单独文件来减少 `.bashrc` 文件中的混乱。这不是一个自动加载的文件，因此你需要在 `.bashrc` 文件中添加 `source ~/.bash_aliases` 命令。

下面展示了我的一些最喜欢的别名和函数。有关更多信息，请参阅我的 [.bash_aliases 文件](https://github.com/learnbyexample/scripting_course/blob/master/.bash_aliases)。

```sh
alias c='clear'
alias p='pwd'
alias e='exit'

alias c1='cd ../'
alias c2='cd ../../'
alias c3='cd ../../../'

alias ls='ls --color=auto'
alias l='ls -ltrhG'
alias la='l -A'

alias grep='grep --color=auto'

# save the last command from history to a reference file
alias sl='fc -ln -1 | sed "s/^\s*//" >> ~/.saved_cmds.txt'
alias slg='< ~/.saved_cmds.txt grep'

# case insensitive file search
# fs search is same as find -iname '*search*'
fs() { find -iname '*'"$1"'*' ; } 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 你可以使用 `source` 命令与 `.bashrc` 或 `.bash_aliases` 文件作为参数，将此类文件中的更改应用到当前 shell 会话中。

### .inputrc

你可以在 `~/.inputrc` 文件中添加自定义按键绑定。有关更多详细信息，请参阅 [bash 手册：Readline 初始化文件](https://www.gnu.org/software/bash/manual/bash.html#Readline-Init-File)。

下面展示了我 `~/.inputrc` 文件中的几个示例：

```sh
$ cat ~/.inputrc
# use up/down arrow to match history based on starting text of the command
"\eA": history-search-backward
"\e[B": history-search-forward
# use history-substring-search-backward and history-substring-search-forward
# if you want to match anywhere in the command line

# ignore case for filename matching and completion
set completion-ignore-case on

# single Tab press will complete if there's only one match
# multiple completions will be displayed otherwise
set show-all-if-ambiguous on 
```

> ![info 你可以使用 `bind -f ~/.inputrc` 或按 `Ctrl+x Ctrl+r` 来将 `.inputrc` 文件中的更改应用到当前 shell 会话中。

### 进一步阅读

+   [合理的 bash 自定义设置](https://mrzool.cc/writing/sensible-bash/)

+   [Shell 配置子文件](https://blog.sanctum.geek.nz/shell-config-subfiles/)

+   [unix.stackexchange: 何时使用别名、函数和脚本](https://unix.stackexchange.com/q/30925/109046)

+   [unix.stackexchange: bashrc 中的 rc 代表什么](https://unix.stackexchange.com/q/3467/109046)

+   [sxhkd 热键守护进程](https://github.com/baskerville/sxhkd)

## Readline 快捷键

引自 [bash 手册：Readline 交互](https://www.gnu.org/software/bash/manual/bash.html#Readline-Interaction)：

> 在交互式会话中，你经常会输入一长串文本，然后才发现第一行上的单词拼写错误。Readline 库为你提供了一套命令，用于在输入文本时对其进行操作，允许你只需修复你的拼写错误，而无需重新输入大多数行。使用这些编辑命令，你可以将光标移动到需要更正的位置，并删除或插入更正的文本。

默认情况下，命令行编辑绑定模仿 [Emacs](https://www.gnu.org/software/emacs/)（一个文本编辑器）。如果你愿意，可以切换到 Vi 模式（另一个文本编辑器）。本节将讨论一些常用的 Emacs 风格的按键绑定。

### Tab 完成功能

Tab 键可以帮助你根据上下文完成命令、别名、文件名等。如果只有一个可能的完成项，它将在单次 Tab 按键时完成。否则，你可以按两次 Tab 键来获取可能的匹配列表（如果有的话）。

使用前面在 .inputrc 部分中看到的 `set show-all-if-ambiguous on` 命令，将单次和双次 Tab 按键合并为单一操作。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 有关更多详细信息，请参阅 [bash 手册：可编程完成](https://www.gnu.org/software/bash/manual/bash.html#Programmable-Completion)。

### 搜索历史

您可以使用 `Ctrl+r` 来搜索命令历史。按下这个键序列后，输入您希望从历史中匹配的字符，然后按 `Esc` 键返回到命令提示符，或者按 `Enter` 键执行命令。

您可以重复按 `Ctrl+r` 来向后移动到匹配条目，按 `Ctrl+s` 来向前移动。如果 `Ctrl+s` 不按预期工作，请参阅 [unix.stackexchange: disable ctrl-s](https://unix.stackexchange.com/q/332791/109046)。

如在 inputrc 部分所述，您可以使用自定义键映射而不是默认选项。

### 移动光标

文档使用 **Meta**（`M-` 前缀）并指出，在许多键盘上此键被标记为 `Alt`。文档还提到，您也可以使用 `Esc` 键进行此类组合。

+   `Alt+b` 将光标移动到当前或上一个单词的开头。

+   `Alt+f` 将光标移动到下一个单词的末尾。

+   `Ctrl+a` 或 `Home` 将光标移动到命令行开头。

+   `Ctrl+e` 或 `End` 将光标移动到命令行末尾。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) `Alt` 和 `Esc` 组合之间的一个区别是，在按住 `Alt` 键的同时可以继续按 `b` 或 `f`。`Esc` 组合是两个不同的按键，而 `Alt` 必须按住以使快捷键生效。

### 删除字符

+   `Alt+Backspace`（或 `Esc+Backspace`）从单词边界向前删除。

+   `Ctrl+w` 从光标位置向前删除到空白边界。

+   `Ctrl+u` 从光标前的字符删除到行首。

+   `Ctrl+k` 从光标位置删除到命令行末尾。

### 清除屏幕

+   `Ctrl+l` 保留输入的内容并清除终端屏幕。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 注意，`Ctrl+l` 不会尝试完全删除滚动回缓冲区。使用 `clear` 命令来完成此操作。

### 交换单词和字符

+   `Alt+t`（或 `Esc+t`）交换前两个单词。

+   `Ctrl+t` 交换前两个字符。

    +   例如，如果您输入了 `sp` 而不是 `ps`，则当光标位于 `sp` 右侧时按 `Ctrl+t`。

### 插入参数

+   `Alt+.`（或 `Esc+.`）插入上一个命令的最后一个参数，多次按击将遍历到倒数第二个命令等。

    +   例如，如果 `cat temp.txt` 是最后一个使用的命令，则按 `Alt+.` 将插入 `temp.txt`。

    +   您还可以使用 `!$` 来表示上一个命令的最后一个参数。

### 进一步阅读

+   [bash 手册：可绑定 Readline 命令](https://www.gnu.org/software/bash/manual/bash.html#Bindable-Readline-Commands)

+   [wiki.archlinux: Readline 的简单介绍](https://wiki.archlinux.org/title/readline)

+   [高效的命令行导航](https://cupfullofcode.com/blog/2013/07/03/efficient-command-line-navigation/index.html)

+   [Bash 界面和 Bash 语言](https://ratfactor.com/slackware/pkgblog/bash)

## 复制粘贴

以下是在终端中显示的复制粘贴操作的快捷键。你可能可以在终端首选项中自定义这些快捷键。

+   `Shift+Ctrl+c` 将高亮部分复制到剪贴板

+   `Shift+Ctrl+v` 粘贴剪贴板内容

+   `Shift+Insert` 粘贴最后高亮的部分（不一定是剪贴板内容）

你也可以按鼠标中键而不是使用`Shift+Insert`快捷键。这不仅仅限于终端，在许多其他应用程序中也适用。你可以使用`xinput`命令来启用/禁用鼠标按钮点击。首先，使用不带任何参数的`xinput`并找到对应你的鼠标的数字。例如，假设设备号是`11`，你可以使用以下命令：

+   `xinput set-button-map 11 1 0 3` 用于禁用中间按钮点击

+   `xinput set-button-map 11 1 2 3` 用于启用中间按钮点击

## 练习

**1)** 你会用哪个命令来显示所有或特定环境变量的名称和值？

**2)** 如果你为已存在的命令（例如`ls`）添加了一个别名，你将如何调用原始命令而不是别名？

**3)** 为什么下面的别名不起作用？你会用哪个代替？

```sh
# doesn't work as expected
$ alias ext='echo "${1##*.}"'
$ ext ip.txt
 ip.txt

# expected output
$ ext ip.txt
txt
$ ext scores.csv
csv
$ ext file.txt.txt
txt 
```

**4)** 你会如何移除当前 shell 会话中的特定别名/函数定义？

```sh
$ alias hw='echo hello world'
$ hw
hello world
# ???
$ hw
hw: command not found

$ hw() { echo hello there ; }
$ hw
hello there
# ???
$ hw
hw: command not found 
```

**5)** 编写一个别名和一个函数，通过将`:`转换为换行符，在单独的行上显示`PATH`环境变量的内容。以下是一个示例输出。

```sh
$ echo "$PATH"
/usr/local/bin:/usr/bin:/bin:/usr/games

# alias
$ a_p
/usr/local/bin
/usr/bin
/bin
/usr/games

# function
$ f_p
/usr/local/bin
/usr/bin
/bin
/usr/games 
```

**6)** 登录 shell 会自动读取并执行`~/.bashrc`吗？

**7)** 如果你希望有无限的历史条目，应该将`HISTSIZE`的值设为多少？

**8)** 绑定`set completion-ignore-case on`的作用是什么？

**9)** 哪个快捷键可以帮助你交互式地搜索命令历史？

**10)** 快捷键`Alt+b`和`Alt+f`的作用是什么？

**11)** `Ctrl+l`快捷键和`clear`命令之间有区别吗？

**12)** 你会用哪个快捷键来删除光标前的字符直到行首？

**13)** 快捷键`Alt+t`和`Ctrl+t`的作用是什么？

**14)** `Shift+Insert`和`Shift+Ctrl+v`快捷键之间有区别吗？
