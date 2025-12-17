# 练习解答

> 原文：[`learnbyexample.github.io/cli-computing/exercise-solutions.html`](https://learnbyexample.github.io/cli-computing/exercise-solutions.html)

## 命令行概述

**1)** 在你的系统中，`echo` 默认是 shell 内建命令还是外部命令？你可以使用什么命令来回答这个问题？

在我的系统中，`echo` 既是 shell 内建命令也是外部命令。

```sh
$ type -a echo
echo is a shell builtin
echo is /bin/echo 
```

如上所示的结果，内建命令具有优先级，因此这是默认版本。

**2)** 对于下面的命令，你会得到什么输出？文档是否有助于理解结果？

```sh
$ echo apple     42 'banana     100'
apple 42 banana     100 
```

是的，文档有助于理解上述结果。从 `help echo`（因为内建版本是默认版本）：

> 在标准输出上显示 ARGs，由单个空格字符分隔，后跟换行符。

在上述命令中，有三个参数传递给 `echo` 命令 — `apple`、`42` 和 `'banana 100'`。这些参数所表示的字符串以单个空格字符分隔显示在输出中。

**3)** 阅读 [bash 手册：波浪号展开](https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html)。`~/projects` 是相对路径还是绝对路径？参见 [这个 unix.stackexchange 线程](https://unix.stackexchange.com/q/221970/109046) 以获取答案。

我并不关心它是否正确地称为相对路径或绝对路径。更重要的是，我想强调上述 unix.stackexchange 线程中的这个陷阱：

> `~` 是 shell（以及其他为了方便而模仿它的程序）实现的一种语法，它将其展开为实际的路径名。为了说明，`~/Documents` 大约等同于 `$HOME/Documents`（再次是 shell 语法）。由于 `$HOME` 应该是一个绝对路径，因此 `$HOME/Documents` 的值也是一个绝对路径。但是文本 `$HOME/Documents` 或 `~/Documents` 必须由 shell 展开，才能成为我们想要的路径。

我花了几小时试图调试为什么我的一个 [自动启动](https://wiki.archlinux.org/title/Autostarting) 脚本不起作用。没错，你猜对了。问题在于使用了 `~` 并将其更改为完整路径后解决了问题。

**4)** 当 `less` 命令激活时，你会使用哪个键来获取帮助？

`h`

**5)** 在查看 `man` 页面时，如何将第 50 行移动到屏幕顶部（假设 `less` 命令是分页器）？

`50g`

**6)** `Ctrl+k` 快捷键的作用是什么？

从当前字符删除到命令行末尾。

**7)** 简要解释以下 shell 操作符的作用：

*a)* `|` — 将一个命令的输出作为输入重定向到另一个命令

*b)* `>` — 将一个命令的输出重定向到文件（如果文件已存在则覆盖）

*c)* `>>` — 将一个命令的输出重定向到文件（如果文件已存在则追加）

**8)** `whatis` 命令显示关于命令的一行描述。但它似乎不适用于 `whatis type`。你应该使用什么代替？

```sh
$ whatis cat
cat (1)              - concatenate files and print on the standard output

$ whatis type
type: nothing appropriate.

# need to use 'help -d' since 'type' is a shell builtin
$ help -d type
type - Display information about command type. 
```

**9)** `/tmp` 目录的作用是什么？

根据 `man hier`：

> 此目录包含可能没有通知就被删除的临时文件，例如由常规作业或系统启动时删除。

查看 [wikipedia: 临时文件夹](https://en.wikipedia.org/wiki/Temporary_folder) 获取更多详细信息。

**10)** 请分别给出绝对路径和相对路径的示例。

+   绝对路径：`/usr/share/dict/words`

+   相对路径：`../../projects`

**11)** 你会在什么情况下使用 `man -k` 命令？

根据 `man man`：

> `-k, --apropos`
> 
> 等同于 apropos。搜索简短的手册页描述中的关键字并显示任何匹配项。有关详细信息，请参阅 apropos(1)。

例如：

```sh
# same as: apropos column
$ man -k column
colrm (1)            - remove columns from a file
column (1)           - columnate lists
git-column (1)       - Display data in columns 
```

**12)** `man` 页面和 `info` 页面之间有区别吗？

Linux 手册页通常是完整文档的简短版本。你可以使用 `info` 命令查看 GNU 工具的完整文档。`info` 也是一个 TUI 应用程序，但与 `man` 命令相比，键配置不同。如果你更喜欢在网页浏览器中阅读，请参阅 [GNU 手册在线](https://www.gnu.org/manual/manual.html)。你也可以下载为 PDF 等格式的离线使用。

## 管理文件和目录

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) `ls.sh` 脚本将被用于一些练习。

**1)** 哪个命令将始终显示主目录的绝对路径？

*a)* `pwd`

*b)* `echo "$PWD"`

*c)* `echo "$HOME"`

答案：*c)* `echo "$HOME"`

**2)** 当前工作目录有一个名为 `-dash` 的文件夹。你将如何切换到该目录？

*a)* `cd -- -dash`

*b)* `cd -dash`

*c)* `cd ./-dash`

*d)* `cd \-dash`

*e)* `cd '-dash'`

*f)* 所有以上选项

*g)* 只有 *a)* 和 *c)*

答案：*g)* 只有 *a)* 和 *c)*

**3)** 给定以下目录结构，你将如何切换到 `todos` 目录？

```sh
# change to the 'scripts' directory and source the 'ls.sh' script
$ source ls.sh

$ ls -F
backups/    hello_world.py*  ip.txt     report.log  todos/
errors.log  hi*              projects/  scripts@
$ cd projects
$ pwd
/home/learnbyexample/cli-computing/example_files/scripts/ls_examples/projects

$ cd ../todos
$ pwd
/home/learnbyexample/cli-computing/example_files/scripts/ls_examples/todos 
```

**4)** 根据以下场景，你将如何切换到用户主目录下的 `cli-computing` 目录？然后，你将如何返回到上一个工作目录？

```sh
$ pwd
/home/learnbyexample/all/projects/square_tictactoe

$ cd ~/cli-computing
$ pwd
/home/learnbyexample/cli-computing

$ cd -
$ pwd
/home/learnbyexample/all/projects/square_tictactoe 
```

**5)** 你会如何列出当前目录的内容，每行一个，同时以可读的格式显示条目的大小？

```sh
# change to the 'scripts' directory and source the 'ls.sh' script
$ source ls.sh

$ ls -1sh
total 7.4M
4.0K backups
 16K errors.log
4.0K hello_world.py
4.0K hi
4.0K ip.txt
4.0K projects
7.4M report.log
   0 scripts
4.0K todos 
```

**6)** 你会使用哪个 `ls` 命令选项来进行基于版本的条目排序？

根据 `man ls`：

> `-v`
> 
> 文本中（版本）数字的自然排序

**7)** 你会使用哪个 `ls` 命令选项来根据条目大小进行排序？

> `-S`
> 
> 按文件大小排序，从大到小

**8)** 你会使用哪个 `ls` 命令选项来根据文件扩展名进行排序？

> `-X`
> 
> 按条目扩展名字母顺序排序

**9)** `ls` 命令的 `-G` 选项做什么？

> `-G, --no-group`
> 
> 在长列表中，不打印组名

**10)** `ls` 命令的 `-i` 选项做什么？

> `-i, --inode`
> 
> 打印每个文件的索引号

**11)** 每行只列出目录。

```sh
# change to the 'scripts' directory and source the 'ls.sh' script
$ source ls.sh

$ ls -1d */
backups/
projects/
scripts/
todos/ 
```

**12)** 假设已经存在一个名为 `notes` 的常规文件。如果你使用 `mkdir -p notes` 命令会发生什么？

```sh
$ ls -1F notes
notes

# what would happen here?
$ mkdir -p notes
mkdir: cannot create directory ‘notes’: File exists 
```

**13)** 使用一个或多个命令来匹配以下场景：

```sh
$ ls -1F
cost.txt

# can also use: mkdir {gho,que,toa}st
# brace expansion is covered in the "Shell Features" chapter
$ mkdir ghost quest toast

$ ls -1F
cost.txt
ghost/
quest/
toast/ 
```

**14)** 使用一个或多个命令来匹配以下场景：

```sh
# start with an empty directory
$ ls -l
total 0

# can also use: mkdir -p hobbies/{painting,trekking,writing} shopping
# or: mkdir -p hobbies/{paint,trekk,writ}ing shopping
$ mkdir -p hobbies/painting hobbies/trekking hobbies/writing shopping
$ touch hobbies/painting/waterfall.bmp hobbies/trekking/himalayas.txt
$ touch shopping/festival.xlsx

$ tree -F
.
├── hobbies/
│   ├── painting/
│   │   └── waterfall.bmp
│   ├── trekking/
│   │   └── himalayas.txt
│   └── writing/
└── shopping/
    └── festival.xlsx

5 directories, 3 files 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 不要删除此目录，稍后练习中需要使用。

**15)** 如果要创建的目录已经存在，你会使用哪个 `mkdir` 命令选项来不显示错误？

> `-p, --parents`
> 
> 如果存在，则不报错，按需创建父目录

**16)** 使用一个或多个命令来匹配以下场景：

```sh
$ ls -1F
cost.txt
ghost/
quest/
toast/

$ rm -r cost.txt ghost toast

$ ls -1F
quest/ 
```

**17)** `rm` 命令的 `-f` 选项做什么？

> `-f, --force`
> 
> 忽略不存在的文件和参数，永不提示

例如，它有助于删除写保护的文件（前提是你有删除这些文件的适当权限）。

**18)** 你会使用哪个选项来交互式地使用 `rm` 命令删除文件？

> `-i`
> 
> 在每次删除前提示
> 
> `-I`
> 
> 在删除超过三个文件之前或递归删除时提示一次；比 `-i` 更不侵入，同时仍然提供对大多数错误的保护

**19)** 使用 `rm` 删除的文件是否可以轻松恢复？你需要采取一些额外步骤或使用特殊命令来使文件更难恢复？

+   使用 `rm` 删除的文件仍然可以通过时间和技巧恢复

    +   [unix.stackexchange: 恢复已删除的文件](https://unix.stackexchange.com/q/80270/109046)

    +   [unix.stackexchange: 恢复意外删除的文件](https://unix.stackexchange.com/q/2677/109046)

+   如果你想使删除的文件更难恢复，可以使用 `shred` 等命令

    +   [wiki.archlinux: 安全擦除磁盘](https://wiki.archlinux.org/title/Securely_wipe_disk)

**20)** 你的 Linux 发行版是否提供将删除的文件发送到垃圾箱的工具（这有助于恢复删除的文件）？

在 Ubuntu 上，你可以使用 `sudo apt install trash-cli` 来安装 `trash` 命令。另请参阅 [wiki.archlinux: 垃圾箱管理](https://wiki.archlinux.org/title/Trash_management)。

**21)** 你会使用哪个选项来交互式地接受/防止 `cp` 命令覆盖同名文件？以及哪个选项可以防止覆盖而无需手动确认？

> `-i, --interactive`
> 
> 在覆盖前提示（覆盖了之前的 -n 选项）
> 
> `-n, --no-clobber`
> 
> 不要覆盖现有文件（覆盖了之前的 -i 选项）

**22)** `cp` 命令是否允许你重命名正在复制的文件或目录？如果是，能否重命名多个正在复制的文件/目录？

`cp` 允许通过在目标路径中指定不同的名称来重命名单个文件或目录。你不能通过单个 `cp` 使用来重命名多个文件或目录。

**23)** `cp` 命令的 `-u`、`-b` 和 `-t` 选项做什么？

> `-u, --update`
> 
> 仅当源文件比目标文件新或目标文件缺失时才复制
> 
> `--backup[=CONTROL]`
> 
> 为每个现有目标文件创建备份
> 
> `-b`
> 
> 与 `--backup` 类似，但不接受参数
> 
> `-t, --target-directory=DIRECTORY`
> 
> 将所有源参数复制到目录中

**24)** 下面两个命令之间的区别是什么？

```sh
$ cp ip.txt op.txt

$ mv ip.txt op.txt 
```

+   `cp` 创建 `ip.txt` 的新副本，命名为 `op.txt` — 两个具有相同内容的文件

+   `mv` 将 `ip.txt` 重命名为 `op.txt` — 只有一个文件

**25)** 你会使用哪个选项来交互式地接受/阻止 `mv` 命令覆盖同名文件？

> `-i, --interactive`
> 
> 在覆盖前提示

**26)** 使用一个或多个命令来匹配以下场景。你应该已经在之前的练习中创建了此目录结构。

```sh
$ tree -F
.
├── hobbies/
│   ├── painting/
│   │   └── waterfall.bmp
│   ├── trekking/
│   │   └── himalayas.txt
│   └── writing/
└── shopping/
    └── festival.xlsx

5 directories, 3 files

$ mv hobbies/*/* hobbies/
$ rm -r hobbies/*/

$ tree -F
.
├── hobbies/
│   ├── himalayas.txt
│   └── waterfall.bmp
└── shopping/
    └── festival.xlsx

2 directories, 3 files 
```

**27)** `mv` 命令的 `-t` 选项做什么？

> `-t, --target-directory=DIRECTORY`
> 
> 将所有源参数移动到目录中

**28)** 根据下面的文件名和预期输出确定并实现 `rename` 逻辑。

```sh
$ touch '(2020) report part 1.txt' 'analysis part 3 (2018).log'
$ ls -1
'(2020) report part 1.txt'
'analysis part 3 (2018).log'

# can also use: rename 's/[()]//g; y/ /_/' *
$ rename 's/ /_/g; s/[()]//g' *

$ ls -1
2020_report_part_1.txt
analysis_part_3_2018.log 
```

**29)** `ln` 命令指定源和目标的方式是否与 `cp` 和 `mv` 命令相同？

是的。

**30)** 哪个 `tar` 选项有助于根据文件扩展名压缩归档？此选项可以用作代替 `-z`（用于 `gzip`），`-j`（用于 `bzip2`）和 `-J`（用于 `xz`）。

> `-a, --auto-compress`
> 
> 使用归档后缀来确定压缩程序。

## Shell 特性

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 `globs.sh` 脚本进行与通配符相关的练习，除非另有说明。
> 
> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) 为可能需要创建一些文件的练习创建一个临时目录。之后可以删除这样的练习目录。

**1)** 使用 `echo` 命令以如下所示显示文本。根据需要使用适当的引号。

```sh
# can also use: echo "that's"'    great! $x = $y + $z'
$ echo 'that'\''s    great! $x = $y + $z'
that's    great! $x = $y + $z 
```

**2)** 使用 `echo` 命令以如下格式显示三个变量的值。

```sh
$ n1=10
$ n2=90
$ op=100

$ echo "$n1 + $n2 = $op"
10 + 90 = 100 
```

**3)** 下面命令的输出将是什么？

```sh
$ echo $'\x22apple\x22: \x2710\x27'
"apple": '10' 
```

**4)** 列出以数字字符开头的文件名。

```sh
# change to the 'scripts' directory and source the 'globs.sh' script
$ source globs.sh

$ ls [0-9]*
100.sh  42.txt 
```

**5)** 列出扩展名不以 `t` 或 `l` 开头的文件名。假设扩展名至少有一个字符。

```sh
# can also use: ls *.[!tl]*
$ ls *.[^tl]*
100.sh  calc.py  hello.py  hi.sh  main.c  math.h 
```

**6)** 列出扩展名只有一个字符的文件名。

```sh
$ ls *.?
main.c  math.h 
```

**7)** 列出扩展名不是 `txt` 的文件名。

```sh
$ shopt -s extglob
$ ls *.!(txt)
100.sh   hello.py  main.c  report-00.log  report-04.log
calc.py  hi.sh     math.h  report-02.log  report-98.log 
```

**8)** 描述下面命令中使用的通配符模式。

```sh
$ ls *[^[:word:]]*.*
report-00.log  report-02.log  report-04.log  report-98.log 
```

列出在 `.` 字符之前至少有一个非单词字符（例如 `-`）的文件。

**9)** 列出扩展名前只有小写字母的文件名。

```sh
$ ls +([a-z]).*
calc.py  hello.py  hi.sh  ip.txt  main.c  math.h  notes.txt 
```

**10)** 列出以 `ma` 或 `he` 或 `hi` 开头的文件名。

```sh
$ ls ma* he* hi*
hello.py  hi.sh  main.c  math.h

# alternate solutions
$ ls @(ma|h[ei])*
$ ls @(ma|he|hi)* 
```

**11)** 你会使用哪些命令来获取以下输出？假设你不知道子目录的深度。

```sh
# change to the 'scripts' directory and source the 'ls.sh' script
$ source ls.sh

# filenames ending with '.txt'
$ shopt -s globstar
$ ls **/*.txt
ip.txt  todos/books.txt  todos/outing.txt

# directories starting with 'c' or 'd' or 'g' or 'r' or 't'
$ ls -1d **/[cdgrt]*/
backups/dot_files/
projects/calculator/
projects/tictactoe/
todos/ 
```

**12)** 创建并切换到一个空目录。然后，使用大括号展开和相关的命令来获取以下结果。

```sh
$ mkdir practice_brace && cd $_
$ touch report_202{0..2}.txt
$ ls report*
report_2020.txt  report_2021.txt  report_2022.txt

# use the 'cp' command here
$ cp report_2021.txt{,.bkp}
$ ls report*
report_2020.txt  report_2021.txt  report_2021.txt.bkp  report_2022.txt 
```

**13)** 内置的 `set` 命令做什么？

根据 `help set` 的说明：

> 更改 shell 属性和位置参数的值，或显示 shell 变量的名称和值。

**14)** `|` 管道运算符的作用是什么？你会在什么情况下添加 `tee` 命令？

`|` 将一个命令的输出作为输入传递给另一个命令。`tee` 命令可以帮助将命令的输出保存到文件中，并在终端上显示。

**15)** 你能推断出以下命令的作用吗？*提示*：参见 `help printf`。

```sh
$ printf '%s\n' apple car dragon
apple
car
dragon 
```

根据 `help printf` 的说明：

> 格式根据需要重复使用，以消耗所有参数。如果参数的数量少于格式要求的数量，额外的格式说明符将表现得像提供了零值或空字符串，具体取决于情况。

在上述示例中，格式 `%s\n` 被应用于所有三个参数。

**16)** 使用大括号展开以及相关的命令和 shell 特性来得到以下结果。*提示*：参见前一个问题。

```sh
$ ls ip.txt
ls: cannot access 'ip.txt': No such file or directory

# can also use: printf '%s\n' item_{10..20..2} > ip.txt
$ printf 'item_%s\n' {10..20..2} > ip.txt
$ cat ip.txt
item_10
item_12
item_14
item_16
item_18
item_20 
```

**17)** 在 `ip.txt` 包含的文本如前一个问题所示的情况下，使用大括号展开和相关的命令来得到以下结果。

```sh
$ printf '%s\n' apple_{1..3}_banana_{6..8} >> ip.txt
$ cat ip.txt
item_10
item_12
item_14
item_16
item_18
item_20
apple_1_banana_6
apple_1_banana_7
apple_1_banana_8
apple_2_banana_6
apple_2_banana_7
apple_2_banana_8
apple_3_banana_6
apple_3_banana_7
apple_3_banana_8 
```

**18)** 如果有的话，`<` 和 `|` shell 运算符之间的区别是什么？

+   `<` 重定向运算符可以帮助您将文件数据作为输入传递给命令

+   `|` 运算符将一个命令的输出作为输入传递给另一个命令

**19)** 通常用哪个字符来表示 `stdin` 数据作为文件参数？

`-`

**20)** 以下运算符的作用是什么？

*a)* `1>` — 将命令的标准输出重定向到文件

*b)* `2>` — 将命令的标准错误重定向到文件

*c)* `&>` — 同时重定向 `stdout` 和 `stderr`（覆盖现有文件）

*d)* `&>>` — 同时重定向 `stdout` 和 `stderr`（追加到现有文件）

*e)* `|&` — 将 `stdout` 和 `stderr` 作为输入传递给另一个命令

**21)** 如果你使用以下 `grep` 命令，`op.txt` 的内容将是什么？

```sh
# press Ctrl+d after the line containing 'histogram'
$ grep 'hi' > op.txt
hi there
this is a sample line
have a nice day
histogram

# you'll get lines containing 'hi'
$ cat op.txt
hi there
this is a sample line
histogram 
```

**22)** 如果你使用以下命令，`op.txt` 的内容将是什么？

```sh
$ qty=42
$ cat << end > op.txt
> dragon
> unicorn
> apple $qty
> ice cream
> end

$ cat op.txt
dragon
unicorn
apple 42
ice cream 
```

注意，`qty` 变量的值被替换为 `$qty`。您必须使用 `'end'` 或 `\end` 来避免 shell 替换。

**23)** 修正以下命令以获得以下预期的输出。

```sh
$ books='cradle piranesi soulhome bastion'

# something is wrong with this command
$ sed 's/\b\w/\u&/g' <<< '$books'
$Books

# double quotes is needed for variable interpolation
$ sed 's/\b\w/\u&/g' <<< "$books"
Cradle Piranesi Soulhome Bastion 
```

**24)** 修正以下命令以获得以下预期的输出。

```sh
# something is wrong with this command
$ echo 'hello' ; seq 3 > op.txt
hello
$ cat op.txt
1
2
3

# can also use: { echo 'hello' ; seq 3 ; } > op.txt
$ (echo 'hello' ; seq 3) > op.txt
$ cat op.txt
hello
1
2
3 
```

**25)** 以下命令的输出将是什么？

```sh
$ printf 'hello' | tr 'a-z' 'A-Z' && echo ' there'
HELLO there

$ printf 'hello' | tr 'a-z' 'A-Z' || echo ' there'
HELLO 
```

在这两种情况下，第一个命令成功（退出状态 `0`）。`&&` 和 `||` 是短路运算符。它们的第二个操作数只有在第一个操作数成功或失败时才会执行。

**26)** 修正以下命令以获得以下预期的输出。

```sh
# something is wrong with these commands
$ nums=$(seq 3)
$ echo $nums
1 2 3

$ echo "$nums"
1
2
3 
```

**27)** 以下两个命令会产生等效的输出吗？如果不，为什么？

```sh
$ paste -d, <(seq 3) <(printf '%s\n' item_{1..3})
1,item_1
2,item_2
3,item_3

$ printf '%s\n' {1..3},item_{1..3}
1,item_1
1,item_2
1,item_3
2,item_1
2,item_2
2,item_3
3,item_1
3,item_2
3,item_3 
```

输出不相等，因为当使用多个大括号时，大括号展开会创建所有组合。

## 查看部分或整个文件内容

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 在以下练习中使用的输入文件请使用 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录。

**1)** 您会使用哪个选项（些）来获取以下输出？

```sh
$ printf '\n\n\ndragon\n\n\nunicorn\n\n\n' | cat -bs

     1  dragon

     2  unicorn 
```

**2)** 向 `cat` 命令传递适当的参数以获取以下输出。

```sh
$ cat greeting.txt
Hi there
Have a nice day

$ echo '42 apples and 100 bananas' | cat - greeting.txt
42 apples and 100 bananas
Hi there
Have a nice day 
```

**3)** 以下两个命令会产生相同的输出吗？如果不，为什么？

```sh
$ cat fruits.txt ip.txt | tac
blue delight
light orange
deep blue
mango
papaya
banana

$ tac fruits.txt ip.txt
mango
papaya
banana
blue delight
light orange
deep blue 
```

不。输出不同是因为 `tac` 分别为每个输入文件反转内容。

**4)** 阅读关于 `tac` 命令的手册，并使用适当的选项和参数以获取以下输出。

```sh
$ cat blocks.txt
%=%=
apple
banana
%=%=
brown
green

$ tac -bs '%=%=' blocks.txt
%=%=
brown
green
%=%=
apple
banana 
```

> `-b, --before`
> 
> 在前面而不是后面附加分隔符
> 
> `-s, --separator=STRING`
> 
> 使用 STRING 作为分隔符而不是换行符

**5)** `less -n` 和 `less -N` 选项之间的区别是什么？`cat -n` 和 `less -n` 是否具有类似的功能？

`less -N` 启用行号，而 `less -n` 禁用行号。`cat -n` 启用行号，因此它不与 `less -n` 功能类似。

**6)** 您会使用哪个命令在现有的 `less` 会话中打开另一个文件？您会使用哪些命令在先前和下一个文件之间导航？

您可以使用 `:e filename` 打开另一个文件（类似于 Vim 文本编辑器）。您可以使用 `:p` 和 `:n` 在上一个文件和下一个文件之间切换。

**7)** 使用适当的命令和 shell 功能以获取以下输出。

```sh
$ printf 'carpet\njeep\nbus\n'
carpet
jeep
bus

# use the above 'printf' command for input data
$ c=$(printf 'carpet\njeep\nbus\n' | head -c3)
$ echo "$c"
car 
```

**8)** 如何显示除第一行之外的所有输入行？

```sh
$ printf 'apple\nfig\ncarpet\njeep\nbus\n' | tail -n +2
fig
carpet
jeep
bus 
```

**9)** 您会使用哪个命令（些）来获取以下输出？

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

$ head -q -n2 fruits.txt blocks.txt
banana
papaya
%=%=
apple 
```

**10)** 使用 `head` 和 `tail` 命令的组合从给定输入中获取第 11 到 14 个字符。

```sh
# can also use: tail -c +11 | head -c4
$ printf 'apple\nfig\ncarpet\njeep\nbus\n' | head -c14 | tail -c +11
carp 
```

**11)** 从输入文件 `table.txt` 和 `fruits.txt` 中提取前六个字节。

```sh
$ head -q -c6 table.txt fruits.txt
brown banana 
```

**12)** 从输入文件 `fruits.txt` 和 `table.txt` 中提取最后六个字节。

```sh
$ tail -q -c6 fruits.txt table.txt
mango
 3.14 
```

## 搜索文件和文件名

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 对于 `grep` 练习，除非另有说明，请使用 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录作为输入文件。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 对于 `find` 练习，除非另有说明，请使用 `find.sh` 脚本。

**1)** 显示来自输入文件 `blocks.txt`、`ip.txt` 和 `uniform.txt` 包含 `an` 的行。显示带有和没有文件名前缀的结果。

```sh
$ grep 'an' blocks.txt ip.txt uniform.txt
blocks.txt:banana
ip.txt:light orange
uniform.txt:mango

$ grep -h 'an' blocks.txt ip.txt uniform.txt
banana
light orange
mango 
```

**2)** 显示 `sample.txt` 输入文件中包含整个单词 `he` 的行。

```sh
$ grep -w 'he' sample.txt
14) He he he 
```

**3)** 仅匹配包含 `car` 的整行，不考虑大小写。匹配的行应显示带有行号前缀。

```sh
$ printf 'car\nscared\ntar car par\nCar\n' | grep -nix 'car'
1:car
4:Car 
```

**4)** 显示 `purchases.txt` 中除包含 `tea` 的行之外的所有行。

```sh
$ grep -v 'tea' purchases.txt
coffee
washing powder
coffee
toothpaste
soap 
```

**5)** 显示 `sample.txt` 中包含 `do` 但不包含 `it` 的所有行。

```sh
# can also use: grep -P '^(?!.*it).*do' sample.txt
$ grep 'do' sample.txt | grep -v 'it'
13) Much ado about nothing 
```

**6)** 对于输入文件`sample.txt`，过滤包含`do`的行，并显示匹配行之后的行。

```sh
$ grep -A1 'do' sample.txt
 6) Just do-it
 7) Believe it
--
13) Much ado about nothing
14) He he he 
```

**7)** 对于输入文件`sample.txt`，过滤包含`are`或`he`作为完整单词的行以及匹配行之前的行。通过`info grep`或[在线手册](https://www.gnu.org/software/grep/manual/grep.html)了解适当的选项，以确保输出中匹配行组之间没有分隔符。

```sh
$ grep --no-group-separator -B1 -wE 'are|he' sample.txt
 3) Hi there
 4) How are you
13) Much ado about nothing
14) He he he 
```

> `--no-group-separator`
> 
> 当使用`-A`、`-B`或`-C`时，不要在行组之间打印分隔符。

**8)** 提取所有包含或不包含文本的`()`对，前提是它们内部不包含`()`字符。

```sh
$ echo 'I got (12) apples' | grep -o '([^()]*)'
(12)

$ echo '((2 +3)*5)=25 and (4.3/2*()' | grep -o '([^()]*)'
(2 +3)
() 
```

**9)** 对于给定的输入，匹配所有以`den`开头或以`ly`结尾的行。

```sh
$ lines='reply\n1 dentist\n2 lonely\neden\nfly away\ndent\n'

$ printf '%b' "$lines" | grep -E '^den|ly$'
reply
2 lonely
dent 
```

**10)** 提取以`s`开头且包含`e`和`t`（顺序不限）的单词。

```sh
$ words='sequoia subtle exhibit sets tests sit store_2'

$ echo "$words" | grep -owP 's(?=\w*t)(?=\w*e)\w+'
subtle
sets
store_2

# alternate solutions, but these won't scale well with more conditions
$ echo "$words" | grep -ow 's\w*t\w*' | grep 'e'
$ echo "$words" | grep -owE 's\w*(t\w*e|e\w*t)\w*' 
```

**11)** 提取所有首尾字符相同的完整单词。

```sh
# can also use: grep -owE '(\w)(\w*\1)?'
$ echo 'oreo not a _oh_ pip RoaR took 22 Pop' | grep -owE '\w|(\w)\w*\1'
oreo
a
_oh_
pip
RoaR
22 
```

**12)** 匹配所有包含`*[5]`字面量的输入行。

```sh
$ printf '4*5]\n(9-2)*[5]\n[5]*3\nr*[5\n' | grep -F '*[5]'
(9-2)*[5] 
```

**13)** 匹配以`hand`开头并立即跟随着`s`、`y`、`le`或没有其他字符的完整行。

```sh
$ lines='handed\nhand\nhandy\nunhand\nhands\nhandle\nhandss\n'

$ printf '%b' "$lines" | grep -xE 'hand([sy]|le)?'
hand
handy
hands
handle 
```

**14)** 输入行有三个或更多由`,`分隔的字段。从第二个字段提取到倒数第二个字段。换句话说，提取除了第一个和最后一个字段之外的字段。

```sh
$ printf 'apple,fig,cherry\ncat,dog,bat\n' | grep -oP ',\K.+(?=,)'
fig
dog

$ echo 'dragon,42,unicorn,3.14,shapeshifter\n' | grep -oP ',\K.+(?=,)'
42,unicorn,3.14 
```

**15)** 递归搜索包含`ello`的文件。

```sh
# change to the 'scripts' directory and source the 'grep.sh' script
$ source grep.sh

$ grep -rl 'ello'
projects/python/hello.py
projects/shell/hello.sh
colors_1
colors_2 
```

**16)** 递归搜索包含`blue`的文件，但不要在`backups`目录内搜索。

```sh
# change to the 'scripts' directory and source the 'grep.sh' script
$ source grep.sh

$ grep -rl --exclude-dir='backups' 'blue'
.hidden
colors_1
colors_2 
```

**17)** 递归搜索包含`blue`的文件，但文件同时包含`teal`时不搜索。

```sh
# change to the 'scripts' directory and source the 'grep.sh' script
$ source grep.sh

$ grep -rlZ 'blue' | xargs -r0 grep -L 'teal'
.hidden
colors_2
backups/color list.txt 
```

**18)** 在`backups`目录中查找所有常规文件。

```sh
# change to the 'scripts' directory and source the 'find.sh' script
$ source find.sh

$ find backups -type f
backups/dot_files/.bashrc
backups/dot_files/.inputrc
backups/dot_files/.vimrc
backups/aug.log
backups/bookmarks.html
backups/jan.log 
```

**19)** 找出扩展名以`p`、`s`或`v`开头的所有常规文件。

```sh
$ find -type f -name '*.[psv]*'
./projects/tictactoe/game.py
./projects/calculator/calc.sh
./hi.sh
./backups/dot_files/.vimrc
./hello_world.py 
```

**20)** 找出所有名称中不包含小写字母`g`到`l`的常规文件。

```sh
# can also use: find -type f ! -name '*[g-l]*'
$ find -type f -not -name '*[g-l]*'
./todos/TRIP.txt
./todos/wow.txt 
```

**21)** 找出所有路径中至少有一个目录名以`p`或`d`开头的常规文件。

```sh
# can also use: find -type f -regex '.*/[pd].*/.*'
$ find -type f -path '*/[pd]*/*'
./projects/tictactoe/game.py
./projects/calculator/calc.sh
./backups/dot_files/.bashrc
./backups/dot_files/.inputrc
./backups/dot_files/.vimrc 
```

**22)** 找出所有名称包含`b`或`d`的目录。

```sh
$ find -type d -name '*[bd]*'
./todos
./backups
./backups/dot_files 
```

**23)** 找出所有隐藏目录。

```sh
# can also use: find -mindepth 1 -type d -name '.*'
$ find -type d -name '.?*'
./projects/.venv 
```

**24)** 找出深度恰好为`2`的所有常规文件。

```sh
$ find -mindepth 2 -maxdepth 2 -type f
./todos/books.txt
./todos/TRIP.txt
./todos/wow.txt
./backups/aug.log
./backups/bookmarks.html
./backups/jan.log 
```

**25)** `find -mtime`和`find -atime`之间的区别是什么？这些选项使用的时间周期是什么？

`m`代表修改时间戳，`a`代表访问时间戳。这些选项与`24`小时周期一起使用。

> `-atime n`
> 
> 文件最后访问时间为`n*24`小时前。当`find`确定文件上次访问的 24 小时周期数时，任何小数部分将被忽略，因此要匹配`-atime +1`，文件至少需要被访问两天。
> 
> `-mtime n`
> 
> 文件数据最后修改时间为`n*24`小时前。参见注释了解`-atime`如何影响文件修改时间的解释。

**26)** 找出所有空常规文件。

```sh
# can also use: find -type f -size 0
$ find -type f -empty
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

**27)** 创建名为`filtered_files`的目录。然后，将所有大于`1`字节但名称不以`.log`结尾的常规文件复制到该目录。

```sh
$ mkdir filtered_files
$ find -type f -size +1c -not -name '*.log' -exec cp -t filtered_files {} +
$ ls -A filtered_files
hello_world.py  .hidden  hi.sh  ip.txt 
```

**28)** 查找所有隐藏文件，但如果它们是之前创建的`filtered_files`目录的一部分，则不查找。

```sh
$ find -type f -not -path './filtered_files/*' -prune -name '.*'
./.hidden
./backups/dot_files/.bashrc
./backups/dot_files/.inputrc
./backups/dot_files/.vimrc 
```

**29)** 删除之前创建的`filtered_files`目录。然后，阅读`find`手册，找出如何仅列出可执行文件。

```sh
$ rm -r filtered_files
$ find -type f -executable
./hi.sh
./hello_world.py 
```

> `-可执行`
> 
> 匹配当前用户可执行的可执行文件和可搜索的目录（在文件名解析意义上）。

**30)** 列出至少一个将`find`输出通过管道传递到`xargs`命令而不是使用`find -exec`选项的使用案例。

如果需要出于性能原因进行并行执行，`xargs -P`（或[parallel](https://www.gnu.org/software/parallel/)命令）可能很有用。

**31)** `locate`命令是如何比等效的`find`命令运行得更快？

来自[unix.stackexchange: find 和 locate 的优缺点](https://unix.stackexchange.com/q/60205/109046)：

> `locate`使用预构建的数据库，该数据库应定期更新，而`find`则遍历文件系统以定位文件。
> 
> 因此，`locate`比`find`快得多，但如果数据库（可以看作是一个缓存）没有更新，则可能不准确（参见`updatedb`命令）。

## 文件属性

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用[example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files)目录作为以下练习中使用的输入文件，除非另有说明。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 为可能需要您创建一些文件和目录的练习创建一个临时目录。您可以在之后删除此类练习目录。

**1)** 将`greeting.txt`输入文件中的行数保存到`lines` shell 变量中。

```sh
$ lines=$(wc -l &LTgreeting.txt)
$ echo "$lines"
2 
```

**2)** 您认为以下命令的输出会是什么？

```sh
$ echo 'dragons:2 ; unicorns:10' | wc -w
3 
```

**3)** 使用适当的选项和参数来获取以下输出。

```sh
$ printf 'apple\nbanana\ncherry' | wc -lc sample.txt -
     15     183 sample.txt
      2      19 -
     17     202 total 
```

**4)** 阅读`wc`手册，并使用适当的选项和参数来获取以下输出。

```sh
$ printf 'greeting.txt\0scores.csv' | wc --files0-from=-
2 6 25 greeting.txt
4 4 70 scores.csv
6 10 95 total 
```

> `--files0-from=F`
> 
> 从文件 F 中读取由 NUL 终止的名称指定的文件中的输入；如果 F 是-，则从标准输入读取名称

**5)** `wc -c`和`wc -m`选项之间的区别是什么？以及您会使用哪个选项来获取最长行长度？

> `-c, --bytes`
> 
> 打印字节数
> 
> `-m, --chars`
> 
> 打印字符数
> 
> `-L, --max-line-length`
> 
> 打印最大显示宽度

**6)** 查找以`.log`结尾的文件名，并以可读格式报告它们的大小。对于第一种情况，使用`find+du`组合，对于第二种情况，使用`ls`命令（带有适当的 shell 功能）。

```sh
# change to the 'scripts' directory and source the 'du.sh' script
$ source du.sh

$ find -type f -name '*.log' -exec du -h {} +
16K     ./projects/errors.log
7.4M    ./report.log

$ shopt -s globstar
$ ls -1sh **/*.log
 16K projects/errors.log
7.4M report.log 
```

**7)** 在当前路径中按`1000`的幂次报告文件/目录的大小，不进入子目录。最后也显示总数。

```sh
# change to the 'scripts' directory and source the 'du.sh' script
$ source du.sh

$ du -sc --si *
50k     projects
7.7M    report.log
8.2k    todos
7.8M    total 
```

**8)** `du --apparent-size`选项的作用是什么？

> `--apparent-size`
> 
> 打印明显大小，而不是磁盘使用情况；尽管明显大小通常较小，但由于文件中的空洞（稀疏文件）、内部碎片、间接块等，它可能更大

**9)** 你将在什么情况下使用 `df` 命令而不是 `du`？哪个 `df` 命令选项将帮助你仅报告感兴趣的特定字段？

`df` 命令提供整个文件系统的空间使用情况，而 `du` 命令对于获取特定文件和目录的空间估计很有用。

```sh
$ whatis du df
du (1)               - estimate file space usage
df (1)               - report file system disk space usage 
```

要仅获取感兴趣的特定字段：

> `--output[=FIELD_LIST]`
> 
> 使用由 FIELD_LIST 定义的输出格式，或者如果省略 FIELD_LIST，则打印所有字段。

**10)** 按照以下格式显示 `scores.csv` 和 `timings.txt` 文件的大小。

```sh
$ stat -c '%n: %s' scores.csv timings.txt
scores.csv: 70
timings.txt: 49 
```

**11)** 哪个 `touch` 选项可以帮助你在文件尚不存在时防止文件创建？

> `-c, --no-create`
> 
> 不创建任何文件

**12)** 假设 `new_file.txt` 在当前工作目录中不存在。下面显示的 `stat` 命令的输出将会是什么？

```sh
$ touch -t '202010052010.05' new_file.txt
$ stat -c '%y' new_file.txt
2020-10-05 20:10:05.000000000 +0530 
```

> `-t STAMP`
> 
> 使用 [[CC]YY]MMDDhhmm[.ss] 而不是当前时间

**13)** 以下 `touch` 命令是否有效？如果是，随后的 `stat` 命令的输出将会是什么？

是的，这是有效的，因为允许多个文件参数。`-r` 选项有助于将给定文件的时间戳详细信息复制到目标文件。

```sh
# change to the 'scripts' directory and source the 'touch.sh' script
$ source touch.sh

$ stat -c '%n: %y' fruits.txt
fruits.txt: 2017-07-13 13:54:03.576055933 +0530

$ touch -r fruits.txt f{1..3}.txt
$ stat -c '%n: %y' f*.txt
f1.txt: 2017-07-13 13:54:03.576055933 +0530
f2.txt: 2017-07-13 13:54:03.576055933 +0530
f3.txt: 2017-07-13 13:54:03.576055933 +0530
fruits.txt: 2017-07-13 13:54:03.576055933 +0530 
```

**14)** 使用适当的选项（s）以获得以下输出。

```sh
$ printf 'αλεπού\n' | file -
/dev/stdin: UTF-8 Unicode text

$ printf 'αλεπού\n' | file -b -
UTF-8 Unicode text 
```

**15)** 以下命令是否有效？如果是，输出将会是什么？

是的，这是有效的。多个斜杠将被视为单个斜杠。在确定要提取的部分之前，任何尾随的斜杠都将被删除。

```sh
$ basename -s.txt ~///test.txt///
test 
```

**16)** 给定 shell 变量 `p` 中的文件路径，你将如何获得下面显示的输出？

```sh
$ p='~/projects/square_tictactoe/python/game.py'
$ dirname $(dirname "$p")
~/projects/square_tictactoe 
```

**17)** 解释以下 `stat` 命令输出中的每个字符的含义。

```sh
$ stat -c '%A' ../scripts/
drwxrwxr-x 
```

显示的 10 个字符与文件类型和权限相关。第一个字符表示 **文件类型**。最常见的是：

+   `-` 普通文件

+   `d` 目录

+   `l` 符号链接

其余九个字符代表三组 **文件权限**，分别为 *用户* (`u`)、*组* (`g`) 和 *其他人* (`o`)，顺序如下。

+   *用户* — 文件所有者

+   *组* — 作为组的一部分具有文件访问权限的用户

+   *其他人* — 除了所有者之外的其他人

**权限参考表：**

| 字符 | 含义 | 值 |
| --- | --- | --- |
| `r` | 读取 | `4` |
| `w` | 写入 | `2` |
| `x` | 执行 | `1` |
| `-` | 没有权限 | `0` |

**18)** 下面显示的第二个 `stat` 命令的输出将会是什么？

```sh
$ touch new_file.txt
$ stat -c '%a %A' new_file.txt
664 -rw-rw-r--

$ chmod 546 new_file.txt
$ stat -c '%a %A' new_file.txt
546 -r-xr--rw- 
```

**19)** 你将如何使用 `mkdir` 命令指定目录权限？

```sh
# instead of this
$ mkdir back_up
$ chmod 750 back_up
$ stat -c '%a %A' back_up
750 drwxr-x---
$ rm -r back_up

# do this
$ mkdir -m 750 back_up
$ stat -c '%a %A' back_up
750 drwxr-x--- 
```

**20)** 将 `book_list.txt` 的文件权限更改为与下面显示的第二个 `stat` 命令的输出相匹配。不要使用数字 `220`，而要用 `rwx` 字符指定更改。

```sh
$ touch book_list.txt
$ stat -c '%a %A' book_list.txt
664 -rw-rw-r--

# can also use: chmod -r book_list.txt
$ chmod =w book_list.txt
$ stat -c '%a %A' book_list.txt
220 --w--w---- 
```

**21)** 将 `test_dir` 的权限更改为与下面显示的第二个 `stat` 命令的输出相匹配。不要使用数字 `757`，而要用 `rwx` 字符指定更改。

```sh
$ mkdir test_dir
$ stat -c '%a %A' test_dir
775 drwxrwxr-x

$ chmod g-w,o+w test_dir
$ stat -c '%a %A' test_dir
757 drwxr-xrwx 
```

## 管理进程

**1)** 你如何调用一个在后台执行的命令？在作业启动后，你会做什么来将其推送到后台？你可以使用哪些命令来跟踪活动作业？

+   在命令后添加一个 `&` 字符将在后台执行它

+   `Ctrl+z`（挂起当前运行作业）后跟 `bg`（将最近挂起的作业推送到后台）

+   `jobs` 或 `ps` 命令有助于跟踪活动作业

**2)** 作业编号旁边的 `+` 和 `-` 符号表示什么？

从 `info bash`（章节 *作业控制基础*）：

> 在与作业相关的输出中（例如，`jobs` 命令的输出），当前作业总是用 `+` 标记，上一个作业用 `-` 标记。

**3)** 在什么情况下你会使用 `fg %n` 和 `bg %n` 而不是分别使用 `fg` 和 `bg`？

从 `info bash`（章节 *作业控制基础*）：

> 在 shell 中有许多引用作业的方法。字符 `%` 引入作业规范（JOBSPEC）。
> 
> 作业编号 `n` 可以称为 `%n`。

**4)** 哪个选项可以帮助你自定义 `ps` 命令所需的输出字段？

> `-o format`
> 
> 用户定义的格式。格式是一个空白分隔或逗号分隔的列表，它提供了一种指定单个输出列的方法。

**5)** `pgrep -a` 和 `pgrep -l` 选项之间的区别是什么？

> `-a, --list-full`
> 
> 列出完整的命令行以及进程 ID。
> 
> `-l, --list-name`
> 
> 列出进程名称以及进程 ID。

**6)** 如果作业编号是 `2`，你会使用 `kill %2` 还是 `kill 2` 来向该进程发送 `SIGTERM`？

`kill %2`

**7)** `Ctrl+c` 快捷键向当前运行进程发送哪个信号？

按 `Ctrl+c` 发送 `SIGINT`（`2`）信号，通常用于终止进程。

**8)** 哪个命令可以帮助你持续监控进程，包括 PID、内存使用等详细信息？

`top`（或类似 `btop` 和 `htop` 的替代品）

**9)** 哪个键可以帮助你在 `top` 会话中操作终止任务？

`k`

**10)** `free` 命令做什么？

```sh
$ whatis free
free (1)             - Display amount of free and used memory in the system 
```

## 多功能文本处理工具

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录中的输入文件进行以下练习。

**1)** 对于给定的输入，将所有 `0xA0` 替换为 `0x50`，将 `0xFF` 替换为 `0x7F`。

```sh
$ printf 'a1:0xA0, a2:0xA0A1\nb1:0xFF, b2:0xBE\n'
a1:0xA0, a2:0xA0A1
b1:0xFF, b2:0xBE

$ printf 'a1:0xA0, a2:0xA0A1\nb1:0xFF, b2:0xBE\n' | sed 's/0xA0/0x50/g; s/0xFF/0x7F/g'
a1:0x50, a2:0x50A1
b1:0x7F, b2:0xBE 
```

**2)** 仅从给定输入中删除第三行。

```sh
$ seq 34 37 | sed '3d'
34
35
37

# alternate solutions
$ seq 34 37 | awk 'NR!=3'
$ seq 34 37 | perl -ne 'print if $.!=3' 
```

**3)** 对于输入文件 `sample.txt`，显示包含 `it` 但不包含 `do` 的所有行。

```sh
$ sed -n '/it/{/do/!p}' sample.txt
 7) Believe it

# alternate solutions
$ awk '/it/ && !/do/' sample.txt
$ perl -ne 'print if /it/ && !/do/' sample.txt 
```

**4)** 对于输入文件 `purchases.txt`，删除包含 `tea` 的所有行。同时，将所有 `coffee` 的出现替换为 `milk`。将更改写回输入文件本身。原始内容应保存到 `purchases.txt.orig`。之后，从该备份文件恢复内容。

```sh
# make the changes
$ sed -i.orig '/tea/d; s/coffee/milk/g' purchases.txt

$ ls purchases*
purchases.txt  purchases.txt.orig
$ cat purchases.txt
milk
washing powder
milk
toothpaste
soap

# restore the contents
$ mv purchases.txt.orig purchases.txt
$ ls purchases*
purchases.txt
$ cat purchases.txt
coffee
tea
washing powder
coffee
toothpaste
tea
soap
tea

# alternate solutions
$ sed -i.orig -n '/tea/b; s/coffee/milk/g; p' purchases.txt
$ perl -i.orig -pe '$_="" if /tea/; s/coffee/milk/g' purchases.txt
$ perl -i.orig -ne 'next if /tea/; s/coffee/milk/g; print' purchases.txt 
```

**5)** 对于输入文件 `sample.txt`，显示从文件开始到第一次出现 `are` 的所有行。

```sh
$ sed '/are/q' sample.txt
 1) Hello World
 2) 
 3) Hi there
 4) How are you

# alternate solutions
$ awk '1; /are/{exit}' sample.txt
$ perl -ne 'print; exit if /are/' sample.txt 
```

**6)** 删除 `uniform.txt` 输入文件中从包含 `start` 的行到包含 `end` 的行的所有行组。

```sh
$ sed '/start/,/end/d' uniform.txt
mango
icecream
how are you
have a nice day
par,far,mar,tar

# alternate solutions
$ awk '/start/{f=1} !f; /end/{f=0}' uniform.txt
$ perl -ne '$f=1 if /start/; print if !$f; $f=0 if /end/' uniform.txt 
```

**7)** 将 `42` 替换为 `[42]`，除非它在单词的边缘。

```sh
$ echo 'hi42bye nice421423 bad42 cool_4242a 42c' | sed 's/\B42\B/[&]/g'
hi[42]bye nice[42]1[42]3 bad42 cool_[42][42]a 42c 
```

**8)** 用 `X` 替换所有以相同单词字符开头和结尾的整个单词。

```sh
# can also use: sed -E 's/\b(\w|(\w)\w*\2)\b/X/g'
$ echo 'oreo not a _oh_ pip RoaR took 22 Pop' | sed -E 's/\b(\w)(\w*\1)?\b/X/g'
X not X X X X took X Pop 
```

**9)** 对于输入文件 `anchors.txt`，将 markdown 锚点转换为以下所示的超级链接。

```sh
$ cat anchors.txt
# &LTa name="regular-expressions">&LT/a>Regular Expressions
## &LTa name="subexpression-calls">&LT/a>Subexpression calls
## &LTa name="the-dot-meta-character">&LT/a>The dot meta character

$ sed -E 's|[^"]+"([^"]+)">&LT/a>(.+)|\2|' anchors.txt
Regular Expressions
Subexpression calls
The dot meta character 
```

**10)** 将所有 `e` 替换为 `3`，除了前两个匹配项。

```sh
$ echo 'asset sets tests site' | sed 's/e/3/3g'
asset sets t3sts sit3

$ echo 'sample item teem eel' | sed 's/e/3/3g'
sample item t33m 33l 
```

**11)** 以下示例字符串使用 `,` 作为分隔符，字段值也可以为空。使用 `sed` 仅替换第三个字段为 `42`。

```sh
$ echo 'lion,,ant,road,neon' | sed 's/[^,]*/42/3'
lion,,42,road,neon

$ echo ',,,' | sed 's/[^,]*/42/3'
,,42, 
```

**12)** 对于输入文件 `table.txt`，计算并显示每行最后字段的数字乘积。考虑空格为该文件的字段分隔符。

```sh
$ cat table.txt
brown bread mat hair 42
blue cake mug shirt -7
yellow banana window shoes 3.14

$ awk 'BEGIN{p = 1} {p *= $NF} END{print p}' table.txt
-923.16

# alternate solutions
$ perl -lane 'BEGIN{$p = 1} {$p *= $F[-1]} END{print $p}' table.txt 
```

**13)** 从每条输入行中提取 `()` 或 `)(` 之间的内容。假设每行中 `()` 字符只会出现一次。

```sh
$ printf 'apple(ice)pie\n(almond)pista\nyo)yoyo(yo\n'
apple(ice)pie
(almond)pista
yo)yoyo(yo

$ printf 'apple(ice)pie\n(almond)pista\nyo)yoyo(yo\n' | awk -F'[()]' '{print $2}'
ice
almond
yoyo 
```

**14)** 对于输入文件 `scores.csv`，以下格式显示 `Name` 和 `Physics` 字段。

```sh
$ cat scores.csv
Name,Maths,Physics,Chemistry
Ith,100,100,100
Cy,97,98,95
Lin,78,83,80

$ awk -F, '{print $1 ":" $3}' scores.csv
Name:Physics
Ith:100
Cy:98
Lin:83

# alternate solutions
$ awk -F, -v OFS=: '{print $1, $3}' scores.csv
$ perl -F, -lane 'print "$F[0]:$F[2]"' scores.csv
$ perl -F, -lane 'print join ":", @F[0,2]' scores.csv 
```

**15)** 按以下格式提取并显示第三和第一个单词。

```sh
$ echo '%whole(Hello)--{doubt}==ado==' | awk -v FPAT='\\w+' '{print $3 ":" $1}'
doubt:whole

$ echo 'just,\joint*,concession_42<=nice' | awk -v FPAT='\\w+' '{print $3 ":" $1}'
concession_42:just

# alternate solutions
$ echo '%whole(Hello)--{doubt}==ado==' | perl -lne '@F = /\w+/g; print "$F[2]:$F[0]"'
$ echo 'just,\joint*,concession_42<=nice' | perl -lne '@F = /\w+/g; print "$F[2]:$F[0]"' 
```

**16)** 对于输入文件 `scores.csv`，添加一个名为 **GP** 的列，该列通过将 50% 的权重分配给数学，以及 25% 分别分配给物理和化学来计算。

```sh
$ awk -F, -v OFS=, '{$(NF+1) = NR==1 ? "GP" : ($2/2 + ($3+$4)/4)} 1' scores.csv
Name,Maths,Physics,Chemistry,GP
Ith,100,100,100,100
Cy,97,98,95,96.75
Lin,78,83,80,79.75 
```

**17)** 从 `para.txt` 输入文件中显示包含任何数字字符的所有段落。

```sh
$ cat para.txt
hi there
how are you

2 apples
12 bananas

blue sky
yellow sun
brown earth

$ awk -v RS= '/[0-9]/' para.txt
2 apples
12 bananas 
```

**18)** 输入具有 ASCII NUL 字符作为记录分隔符。将其更改为以下所示的点和新行字符。

```sh
$ printf 'apple\npie\0banana\ncherry\0' | awk -v RS='\0' -v ORS='.\n' '1'
apple
pie.
banana
cherry. 
```

**19)** 对于输入文件 `sample.txt`，只有当在两行之前找到 `you` 时，才打印包含 `do` 的匹配行。例如，如果 `do` 在第 10 行，而第 8 行包含 `you`，则应打印第 10 行。

```sh
$ awk 'p2 ~ /you/ && /do/; {p2=p1; p1=$0}' sample.txt
 6) Just do-it

# alternate solutions
$ perl -ne 'print if $p2 =~ /you/ && /do/; $p2=$p1; $p1=$_' sample.txt 
```

**20)** 对于输入文件 `blocks.txt`，从包含 `%=%=` 的行提取内容，直到但不包括下一个这样的行。要提取的块由通过 `-v` 选项传递的变量 `n` 指示。

```sh
$ cat blocks.txt
%=%=
apple
banana
%=%=
brown
green

$ awk -v n=1 '$0 == "%=%="{c++} c==n' blocks.txt
%=%=
apple
banana
$ awk -v n=2 '$0 == "%=%="{c++} c==n' blocks.txt
%=%=
brown
green 
```

**21)** 使用 `awk` 命令显示 `c1.txt` 中存在但不在 `c2.txt` 中的行。

```sh
$ awk 'NR==FNR{a[$0]; next} !($0 in a)' c2.txt c1.txt
Brown
Purple
Teal 
```

**22)** 通过根据 `names.txt` 文件中的名称列表匹配第一个字段来显示 `scores.csv` 中的行。

```sh
$ printf 'Ith\nLin\n' > names.txt

$ awk -F, 'NR==FNR{a[$1]; next} $1 in a' names.txt scores.csv
Ith,100,100,100
Lin,78,83,80

$ rm names.txt 
```

**23)** 仅保留 `duplicates.txt` 输入文件中重复行的第一个副本。仅使用最后字段的内容来确定重复项。

```sh
$ cat duplicates.txt
brown,toy,bread,42
dark red,ruby,rose,111
blue,ruby,water,333
dark red,sky,rose,555
yellow,toy,flower,333
white,sky,bread,111
light red,purse,rose,333

$ awk -F, '!seen[$NF]++' duplicates.txt
brown,toy,bread,42
dark red,ruby,rose,111
blue,ruby,water,333
dark red,sky,rose,555

# alternate solutions
$ perl -F, -lane 'print if !$seen{$F[-1]}++' duplicates.txt 
```

**24)** 对于输入文件 `table.txt`，如果第二个字段以 `b` 开头，则打印输入行。使用 `awk` 和 `perl` 构建解决方案。

```sh
$ awk '$2 ~ /^b/' table.txt
brown bread mat hair 42
yellow banana window shoes 3.14

$ perl -lane 'print if $F[1] =~ /^b/' table.txt
brown bread mat hair 42
yellow banana window shoes 3.14 
```

**25)** 对于输入文件 `table.txt`，仅保留倒数第二个字段。将更改写回输入文件本身。原始内容应保存到 `table.txt.bkp`。之后，从该备份文件恢复内容。

```sh
# make the changes
$ perl -i.bkp -lane 'print $F[-2]' table.txt
$ ls table*
table.txt  table.txt.bkp
$ cat table.txt
hair
shirt
shoes

# restore the contents
$ mv table.txt.bkp table.txt
$ ls table*
table.txt
$ cat table.txt
brown bread mat hair 42
blue cake mug shirt -7
yellow banana window shoes 3.14 
```

**26)** 反转 `table.txt` 输入文件的第一字段内容。

```sh
$ perl -lane '$F[0] = reverse $F[0]; print "@F"' table.txt
nworb bread mat hair 42
eulb cake mug shirt -7
wolley banana window shoes 3.14 
```

**27)** 按字典顺序对给定的逗号分隔输入进行排序。将输出字段分隔符更改为冒号字符。

```sh
$ ip='floor,bat,to,dubious,four'
$ echo "$ip" | perl -F, -lane 'print join ":", sort @F'
bat:dubious:floor:four:to 
```

**28)** 过滤包含数字字符的字段。

```sh
$ ip='5pearl 42 east 1337 raku_6 lion 3.14'
$ echo "$ip" | perl -lane 'print join " ", grep {/\d/} @F'
5pearl 42 1337 raku_6 3.14 
```

**29)** 以下输入中有几个以数字字符结尾的单词。将包含 `test` 的单词更改为以下输出所示。也就是说，将匹配部分重新编号为 `1`、`2` 等。不包含 `test` 的单词不应更改。

```sh
$ ip='test_12:test123\nanother_test_4,no_42\n'
$ printf '%b' "$ip"
test_12:test123
another_test_4,no_42

$ printf '%b' "$ip" | perl -pe 's/test\w*?\K\d+/++$i/ge'
test_1:test2
another_test_3,no_42 
```

**30)** 对于输入文件 `table.txt`，将第三字段的内容更改为全部大写。使用 `sed`、`awk` 和 `perl` 构建解决方案。

```sh
$ sed 's/[^ ]*/\U&/3' table.txt
brown bread MAT hair 42
blue cake MUG shirt -7
yellow banana WINDOW shoes 3.14

$ awk '{$3 = toupper($3)} 1' table.txt
brown bread MAT hair 42
blue cake MUG shirt -7
yellow banana WINDOW shoes 3.14

$ perl -lane '$F[2] = uc $F[2]; print "@F"' table.txt
brown bread MAT hair 42
blue cake MUG shirt -7
yellow banana WINDOW shoes 3.14 
```

## 排序内容

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用以下练习中使用的输入文件所在的 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录。

**1)** 默认的 `sort` 不适用于数字。更正以下使用的命令：

```sh
# wrong output
$ printf '100\n10\n20\n3000\n2.45\n' | sort
10
100
20
2.45
3000

# expected output
$ printf '100\n10\n20\n3000\n2.45\n' | sort -n
2.45
10
20
100
3000 
```

**2)** 哪个 `sort` 选项可以帮助你忽略大小写？

```sh
$ printf 'Super\nover\nRUNE\ntea\n' | LC_ALL=C sort -f
over
RUNE
Super
tea 
```

**3)** 阅读`sort`手册，并使用适当的选项来获取以下输出。

```sh
# wrong output
$ printf '+120\n-1.53\n3.14e+4\n42.1e-2' | sort -n
-1.53
+120
3.14e+4
42.1e-2

# expected output
$ printf '+120\n-1.53\n3.14e+4\n42.1e-2' | sort -g
-1.53
42.1e-2
+120
3.14e+4 
```

> `-g, --general-numeric-sort`
> 
> 根据一般数值进行比较

**4)** 使用第二字段的内容按数值升序排序 `scores.csv` 文件。应保留标题行作为第一行，如下所示。*提示*：参见 Shell 特性章节。

```sh
$ (sed -u '1q' ; sort -t, -k2,2n) < scores.csv
Name,Maths,Physics,Chemistry
Lin,78,83,80
Cy,97,98,95
Ith,100,100,100 
```

**5)** 按第四列数字降序排序 `duplicates.txt` 的内容。仅保留具有相同数字的第一行副本。

```sh
$ sort -t, -k4,4nr -u duplicates.txt
dark red,sky,rose,555
blue,ruby,water,333
dark red,ruby,rose,111
brown,toy,bread,42 
```

**6)** 如果输入未排序，`uniq` 会抛出错误吗？你认为以下输入的输出会是什么？

`uniq` 不一定需要输入已排序。相邻行用于比较目的。

```sh
$ printf 'red\nred\nred\ngreen\nred\nblue\nblue' | uniq
red
green
red
blue 
```

**7)** 仅保留基于输入行前两个字符的唯一条目。如有必要，对输入进行排序。

```sh
$ printf '3) cherry\n1) apple\n2) banana\n1) almond\n'
3) cherry
1) apple
2) banana
1) almond

$ printf '3) cherry\n1) apple\n2) banana\n1) almond\n' | sort | uniq -u -w2
2) banana
3) cherry 
```

**8)** 计算输入行重复的次数，并按以下格式显示结果。

```sh
$ printf 'brown\nbrown\nbrown\ngreen\nbrown\nblue\nblue' | sort | uniq -c | sort -n
      1 green
      2 blue
      4 brown 
```

**9)** 使用 `comm` 命令显示 `c1.txt` 中存在但不在 `c2.txt` 中的行。假设输入文件已经排序。

```sh
# can also use: comm -13 c2.txt c1.txt
$ comm -23 c1.txt c2.txt
Brown
Purple
Teal 
```

**10)** 使用适当的选项来获取以下预期的输出。

```sh
# wrong usage, no output
$ join <(printf 'apple 2\nfig 5') <(printf 'Fig 10\nmango 4')

# expected output
$ join -i <(printf 'apple 2\nfig 5') <(printf 'Fig 10\nmango 4')
fig 5 10 
```

**11)** 如果有的话，`sort -u` 和 `uniq -u` 选项之间有什么区别？

`sort -u` 保留被认为是重复的第一份副本。`uniq -u` 仅保留唯一副本（即，重复的副本甚至不会成为输出的一部分）。

## 比较文件

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用以下练习中使用的输入文件所在的 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录。

**1)** 如果你只需要退出状态来反映给定的输入是否相同，你会使用哪个 `cmp` 选项？

> `-s, --quiet, --silent`
> 
> 抑制所有正常输出

**2)** 你会使用哪个 `cmp` 选项来跳过比较目的的初始字节？以下示例需要跳过前两个字节。

```sh
$ echo '1) apple' > x1.txt
$ echo '2\. apple' > x2.txt
$ cmp x1.txt x2.txt
x1.txt x2.txt differ: byte 1, line 1

$ cmp -i2 x1.txt x2.txt
$ echo $?
0

$ rm x[12].txt 
```

> `-i, --ignore-initial=SKIP`
> 
> 跳过两个输入的前 SKIP 个字节

**3)** `diff -d` 选项的作用是什么？

> `-d, --minimal`
> 
> 努力寻找一组更小的更改

**4)** 哪个选项可以帮助你使用 `diff` 获取彩色输出？

> `--color[=WHEN]`
> 
> 着色输出；WHEN 可以是 `never`、`always` 或 `auto`（默认值）

**5)** 使用适当的选项获取以下预期的输出。

```sh
# instead of this output
$ diff -W 40 --suppress-common-lines -y f1.txt f2.txt
2                  |    hello
world              |    4

# get this output
$ diff -W 40 --left-column -y f1.txt f2.txt
1                  (
2                  |    hello
3                  (
world              |    4 
```

> `--left-column`
> 
> 仅输出公共行的左侧列

**6)** 使用适当的选项获取以下预期的输出。

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
$ diff -si d1.txt d2.txt
Files d1.txt and d2.txt are identical

$ rm d[12].txt 
```

## 各种文本处理工具

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录中的输入文件进行以下练习。

**1)** 生成以下序列。

```sh
$ seq 100 -5 80
100
95
90
85
80 
```

**2)** 下面的序列可以用 `seq` 生成吗？如果是，怎么做？

```sh
$ seq -w -s, 01.5 6
01.5,02.5,03.5,04.5,05.5 
```

**3)** 显示来自 `/usr/share/dict/words`（或等效的词典单词文件）中包含 `s`、`e` 和 `t` 的任意顺序的三个随机单词。下面的输出只是一个示例。

```sh
# can also use: grep 's' /usr/share/dict/words | grep 'e' | grep 't' | shuf -n3
$ grep -P '^(?=.*s)(?=.*e).*t' /usr/share/dict/words | shuf -n3
supplemental
foresight
underestimates 
```

**4)** 简要描述 `shuf` 命令选项 `-i`、`-e` 和 `-r` 的用途。

> `-i, --input-range=LO-HI`
> 
> 将 LO 到 HI 之间的每个数字视为一个输入行
> 
> `-e, --echo`
> 
> 将每个 ARG 视为一个输入行
> 
> `-r, --repeat`
> 
> 输出行可以重复

**5)** 为什么以下命令没有按预期工作？在这种情况下，你可以使用哪些其他工具？

`cut` 忽略所有重复字段，输出字段顺序始终与输入字段顺序相同。

```sh
# not working as expected
$ echo 'apple,banana,cherry,dates' | cut -d, -f3,1,3
apple,cherry

# expected output
$ echo 'apple,banana,cherry,dates' | awk -F, -v OFS=, '{print $3, $1, $3}'
cherry,apple,cherry

# alternate solutions
$ echo 'apple,banana,cherry,dates' | perl -F, -lane 'print join ",", @F[2,0,2]' 
```

**6)** 显示除了第二个字段之外的内容，格式如下。你能构造两种不同的解决方案吗？

```sh
$ echo 'apple,banana,cherry,dates' | cut -d, --output-delimiter=' ' -f1,3-
apple cherry dates

$ echo '2,3,4,5,6,7,8' | cut -d, --output-delimiter=' ' --complement -f2
2 4 5 6 7 8 
```

**7)** 从输入行中提取前三个字符，如下所示。你也能使用 `head` 命令来完成这个任务吗？如果不能，为什么？

```sh
$ printf 'apple\nbanana\ncherry\ndates\n' | cut -c-3
app
ban
che
dat 
```

`head` 不能使用，因为它作用于整个输入，而 `cut` 是按行工作的。

**8)** 显示 `scores.csv` 输入文件的第一列和第三列，格式如下。注意，两个列之间只有空格字符，没有制表符。

```sh
$ cat scores.csv
Name,Maths,Physics,Chemistry
Ith,100,100,100
Cy,97,98,95
Lin,78,83,80

$ cut -d, -f1,3 scores.csv | column -s, -t
Name  Physics
Ith   100
Cy    98
Lin   83 
```

**9)** 以以下格式显示 `table.txt` 的内容。

```sh
$ column -t table.txt
brown   bread   mat     hair   42
blue    cake    mug     shirt  -7
yellow  banana  window  shoes  3.14 
```

**10)** 使用 `tr` 命令实现 [ROT13](https://en.wikipedia.org/wiki/ROT13) 密码。

```sh
$ echo 'Hello World' | tr 'a-zA-Z' 'n-za-mN-ZA-M'
Uryyb Jbeyq

$ echo 'Uryyb Jbeyq' | tr 'a-zA-Z' 'n-za-mN-ZA-M'
Hello World 
```

**11)** 仅保留字母、数字和空白字符。

```sh
$ echo 'Apple_42 cool,blue Dragon:army' | tr -dc '[:alnum:][:space:]'
Apple42 coolblue Dragonarmy 
```

**12)** 使用 `tr` 获取以下输出。

```sh
$ echo '!!hhoowwww !!aaaaaareeeeee!! yyouuuu!!' | tr -sd '!' 'a-z'
how are you 
```

**13)** `paste -s` 对多个输入文件分别工作。如果你需要将所有输入文件视为单个源，你会如何解决这个问题？

```sh
# this works individually for each input file
$ paste -sd, fruits.txt ip.txt
banana,papaya,mango
deep blue,light orange,blue delight

# expected output
$ cat fruits.txt ip.txt | paste -sd,
banana,papaya,mango,deep blue,light orange,blue delight

# alternate solutions
$ awk '{printf s $0; s=","} END{print ""}' fruits.txt ip.txt 
```

**14)** 使用适当的选项获取以下预期的输出。

```sh
# default output
$ paste fruits.txt ip.txt
banana  deep blue
papaya  light orange
mango   blue delight

# expected output
$ paste -d'\n' fruits.txt ip.txt
banana
deep blue
papaya
light orange
mango
blue delight 
```

**15)** 使用 `pr` 命令获取以下预期的输出。

```sh
$ seq -w 16 | pr -4ats,
01,02,03,04
05,06,07,08
09,10,11,12
13,14,15,16

$ seq -w 16 | pr -4ts,
01,05,09,13
02,06,10,14
03,07,11,15
04,08,12,16 
```

**16)** 使用 `pr` 命令将输入文件 `fruits.txt` 和 `ip.txt` 连接起来，如下所示。

```sh
$ pr -mts' : ' fruits.txt ip.txt
banana : deep blue
papaya : light orange
mango : blue delight 
```

**17)** `cut` 命令不支持选择最后 `N` 个字段的方法。本章中介绍的哪个工具可以与 `cut` 结合使用以获得以下输出？

```sh
# last two characters from each line
$ printf 'apple\nbanana\ncherry\ndates\n' | rev | cut -c-2 | rev
le
na
ry
es

# alternate solutions
$ printf 'apple\nbanana\ncherry\ndates\n' | grep -o '..$' 
```

**18)** 阅读关于 `split` 的文档，并使用适当的选项来获取以下输入文件 `purchases.txt` 的输出。

```sh
# split input by 3 lines (max) at a time
$ split -l3 purchases.txt

$ head xa?
==> xaa <==
coffee
tea
washing powder

==> xab <==
coffee
toothpaste
tea

==> xac <==
soap
tea

$ rm xa? 
```

> `-l, --lines=NUMBER`
> 
> 每个输出文件中放置 NUMBER 行/记录

**19)** 阅读关于 `split` 的文档，并使用适当的选项来获取以下输出。

```sh
$ echo 'apple,banana,cherry,dates' | split -t, -l1

$ head xa?
==> xaa <==
apple,
==> xab <==
banana,
==> xac <==
cherry,
==> xad <==
dates

$ rm xa? 
```

> `-t, --separator=SEP`
> 
> 使用 SEP 而不是换行符作为记录分隔符；`\0`（零）指定了空字符

**20)** 将输入文件 `purchases.txt` 分割，使得包含 `powder` 行之前的文本是第一个文件的一部分，其余的是第二个文件的一部分，如下所示。

```sh
$ csplit -q purchases.txt '/powder/'

$ head xx0?
==> xx00 <==
coffee
tea

==> xx01 <==
washing powder
coffee
toothpaste
tea
soap
tea

$ rm xx0? 
```

**21)** 编写一个通用的解决方案，用于转换逗号分隔的数据。以下是一个示例输入/输出。你可以使用本书中介绍的任何工具。

```sh
$ cat scores.csv
Name,Maths,Physics,Chemistry
Ith,100,100,100
Cy,97,98,95
Lin,78,83,80

$ tr ',' '\n' &LTscores.csv | pr -$(wc -l &LTscores.csv)ts,
Name,Ith,Cy,Lin
Maths,100,97,78
Physics,100,98,83
Chemistry,100,95,80

# alternate solution if you have GNU datamash installed
$ datamash -t, transpose &LTscores.csv 
```

参见我的博客文章 [使用 GNU datamash 进行 CLI 计算](https://learnbyexample.github.io/cli-computation-gnu-datamash/)。

**22)** 将 `table.txt` 的内容重塑为以下预期的输出。

```sh
$ cat table.txt
brown bread mat hair 42
blue cake mug shirt -7
yellow banana window shoes 3.14

$ xargs -a table.txt -n4 | column -t
brown   bread  mat     hair
42      blue   cake    mug
shirt   -7     yellow  banana
window  shoes  3.14 
```

## Shell 脚本

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 在尝试练习之前，请使用临时工作目录。之后你可以删除这样的练习目录。

**1)** 以下脚本有什么问题？另外，如果你使用 `bash try.sh` 调用脚本，错误会消失吗？

`!#` 应该是 `#!`。如果你不确定应该使用哪一个，请记住，shebang 是一个在脚本开头被特别处理的 **注释**。而且，如果你使用 `bash` 命令调用脚本，错误不会消失。

```sh
$ printf '    \n!#/bin/bash\n\necho hello\n' > try.sh
$ chmod +x try.sh
$ ./try.sh
./try.sh: line 2: !#/bin/bash: No such file or directory
hello

# expected output
$ printf '    \n#!/bin/bash\n\necho hello\n' > try.sh
$ ./try.sh
hello 
```

**2)** 以下命令会工作吗？如果是，输出会是什么？

是的，它会工作。`echo hello` 正在被作为要由 `bash` 命令执行的脚本传递。

```sh
$ echo echo hello | bash
hello 
```

**3)** 你会在什么情况下使用 `source` 脚本而不是使用 `bash` 或通过 shebang 创建可执行文件？

使用 `source` 执行脚本有助于你希望在当前 shell 环境中工作，而不是在子 shell 中。

**4)** 你会如何显示带有 `shake` 后缀的变量的内容？

```sh
$ fruit='banana'

$ echo "${fruit}shake"
bananashake 
```

**5)** 你会对以下代码进行哪些修改以获得预期的输出？

```sh
# default behavior
$ n=100
$ n+=100
$ echo "$n"
100100

# expected output
$ declare -i n=100
$ n+=100
$ echo "$n"
200 
```

**6)** 以下代码是否有效？如果是，`echo` 命令的输出会是什么？

是的，这是有效的。数组索引可以任意使用，它们不必是连续的。

```sh
$ declare -a colors
$ colors[3]='green'
$ colors[1]='blue'

$ echo "${colors[@]}"
blue green 
```

**7)** 你会如何获取变量内容的最后三个字符？

```sh
$ fruit='banana'

$ echo "${fruit: -3}"
ana 
```

**8)** 第二个 `echo` 命令会出错吗？如果不，输出会是什么？

没有错误。它会给出索引 `0` 处元素的长度。

```sh
$ fruits=('apple' 'fig' 'mango')
$ echo "${#fruits[@]}"
3

$ echo "${#fruits}"
5 
```

**9)** 对于给定的数组，使用参数扩展来删除直到第一个/最后一个空格的字符。

```sh
$ colors=('green' 'dark brown' 'deep sky blue white')

# remove till the first space
$ printf '%s\n' "${colors[@]#* }"
green
brown
sky blue white

# remove till the last space
$ printf '%s\n' "${colors[@]##* }"
green
brown
white 
```

**10)** 使用参数扩展来获取以下预期的输出。

```sh
$ ip='apple:banana:cherry:dragon'

$ echo "${ip%:*}"
apple:banana:cherry

$ echo "${ip%%:*}"
apple 
```

**11)** 是否可以使用参数扩展来实现以下预期的输出？如果是，如何？

这是可能的。对于第二和第三种情况，必须启用 `extglob`。

```sh
$ ip1='apple:banana:cherry:dragon'
$ ip2='Cradle:Mistborn:Piranesi'

$ echo "${ip1/:*:/ 42 }"
apple 42 dragon
$ echo "${ip2/:*:/ 42 }"
Cradle 42 Piranesi

$ shopt -s extglob
$ echo "${ip1/#+([^:])/fig}"
fig:banana:cherry:dragon
$ echo "${ip2/#+([^:])/fig}"
fig:Mistborn:Piranesi

$ echo "${ip1/%+([^:])/end}"
apple:banana:cherry:end
$ echo "${ip2/%+([^:])/end}"
Cradle:Mistborn:end 
```

**12)** 对于给定的输入，按照以下显示的预期输出更改大小写。

```sh
$ ip='This is a Sample STRING'

$ echo "${ip^^}"
THIS IS A SAMPLE STRING

$ echo "${ip,,}"
this is a sample string

$ echo "${ip~~}"
tHIS IS A sAMPLE string 
```

**13)** 为什么以下显示的条件表达式失败？

```sh
$ touch ip.txt
$ [[-f ip.txt]] && echo 'file exists'
[[-f: command not found

# need to use space after [[ and before ]]
$ [[ -f ip.txt ]] && echo 'file exists'
file exists 
```

**14)** `==` 和 `=~` 字符串比较操作符之间的区别是什么？

+   `s1 = s2` 或 `s1 == s2` 检查两个字符串是否相等

    +   在测试 `s1` 时，`s2` 的未引用部分将被视为通配符

+   `s1 =~ s2` 检查 `s1` 是否与 `s2` 提供的 POSIX 扩展正则表达式匹配

**15)** 为什么以下使用的条件表达式两次都显示 `failed`？修改表达式，使第一个正确地说 `matched` 而不是 `failed`。

引用部分将被视为字面字符串。通配符应未引用。

```sh
$ f1='1234.txt'
$ f2='report_2.txt'

$ [[ $f1 == '+([0-9]).txt' ]] && echo 'matched' || echo 'failed'
failed
$ [[ $f2 == '+([0-9]).txt' ]] && echo 'matched' || echo 'failed'
failed

# corrected code
$ [[ $f1 == +([0-9]).txt ]] && echo 'matched' || echo 'failed'
matched
$ [[ $f2 == +([0-9]).txt ]] && echo 'matched' || echo 'failed'
failed 
```

**16)** 提取给定变量内容中 `:` 字符后面的数字。

```sh
$ item='chocolate:50'
$ [[ $item =~ :([0-9]+) ]] && echo "${BASH_REMATCH[1]}"
50

$ item='50 apples, fig:100, books-12'
$ [[ $item =~ :([0-9]+) ]] && echo "${BASH_REMATCH[1]}"
100 
```

**17)** 修改以下表达式，使其正确报告 `true` 而不是 `false`。

```sh
$ num=12345
$ [[ $num > 3 ]] && echo 'true' || echo 'false'
false

# corrected code
$ [[ $num -gt 3 ]] && echo 'true' || echo 'false'
true

# alternate solutions
$ (( num > 3 )) && echo 'true' || echo 'false' 
```

**18)** 编写一个名为 `array.sh` 的 shell 脚本，该脚本接受用户输入的数组，然后是另一个输入作为索引。显示该索引处的相应值。以下是一些示例。

```sh
$ cat array.sh
read -p 'enter array elements: ' -a arr
read -p 'enter array index: ' idx
echo "element at index '$idx' is: ${arr[$idx]}"

$ bash array.sh
enter array elements: apple banana cherry
enter array index: 1
element at index '1' is: banana

$ bash array.sh
enter array elements: dragon unicorn centaur
enter array index: -1
element at index '-1' is: centaur 
```

**19)** 编写一个名为 `case.sh` 的 shell 脚本，该脚本接受恰好两个命令行参数。第一个参数可以是 `lower`、`upper` 或 `swap`，并应用于转换第二个参数的内容。以下是一些脚本调用示例，包括如果命令行参数不符合脚本预期会发生什么。

```sh
$ cat case.sh
if (( $# != 2 )) ; then
    echo 'Error! Two arguments expected.' 1>&2
    exit 1
else
    if [[ $1 == 'upper' ]] ; then
        echo "${2^^}"
    elif [[ $1 == 'lower' ]] ; then
        echo "${2,,}"
    elif [[ $1 == 'swap' ]] ; then
        echo "${2~~}"
    else
        echo "Error! '$1' command not recognized." 1>&2
        exit 1
    fi
fi

$ chmod +x case.sh

$ ./case.sh upper 'how are you?'
HOW ARE YOU?

$ ./case.sh lower PineAPPLE
pineapple

$ ./case.sh swap 'HeLlo WoRlD'
hElLO wOrLd

$ ./case.sh lower
Error! Two arguments expected.
$ echo $?
1

$ ./case.sh upper apple fig
Error! Two arguments expected.

$ ./case.sh lowercase DRAGON
Error! 'lowercase' command not recognized.
$ echo $?
1

$ ./case.sh apple lower 2> /dev/null
$ echo $?
1 
```

**20)** 编写一个名为 `loop.sh` 的 shell 脚本，该脚本显示每个作为命令行参数传递的文件的行数。

```sh
$ printf 'apple\nbanana\ncherry\n' > items_1.txt
$ printf 'dragon\nowl\nunicorn\ntroll\ncentaur\n' > items_2.txt

$ cat loop.sh
for file in "$@"; do
    echo "number of lines in '$file' is:" $(wc -l < "$file")
done

$ bash loop.sh items_1.txt
number of lines in 'items_1.txt' is: 3

$ bash loop.sh items_1.txt items_2.txt
number of lines in 'items_1.txt' is: 3
number of lines in 'items_2.txt' is: 5 
```

**21)** 编写一个名为 `read_file.sh` 的 shell 脚本，该脚本逐行读取文件并将其作为参数传递给 `paste -sd,` 命令。你能否使用 `xargs` 命令而不是脚本编写一个解决方案？

```sh
$ printf 'apple\nbanana\ncherry\n' > items_1.txt
$ printf 'dragon\nowl\nunicorn\ntroll\ncentaur\n' > items_2.txt
$ printf 'items_1.txt\nitems_2.txt\n' > list.txt

$ cat read_file.sh
while IFS= read -r line; do
    paste -sd, "$line"
done < "$1"

$ bash read_file.sh list.txt
apple,banana,cherry
dragon,owl,unicorn,troll,centaur

# note that -n1 is not necessary here due to how paste works for multiple files
# but -n1 is necessary to be equivalent to the shell script shown above
$ xargs -a list.txt -d'\n' -n1 paste -sd,
apple,banana,cherry
dragon,owl,unicorn,troll,centaur 
```

**22)** 编写一个名为 `add_path` 的函数，该函数将当前工作目录的路径添加到它接收的参数之前，并显示结果。以下是一些示例。

```sh
$ add_path() { echo "${@/#/$PWD/}" ; }

$ cd
$ pwd
/home/learnbyexample
$ add_path ip.txt report.log
/home/learnbyexample/ip.txt /home/learnbyexample/report.log

$ cd cli-computing
$ pwd
/home/learnbyexample/cli-computing
$ add_path f1
/home/learnbyexample/cli-computing/f1 
```

**23)** `bash -x` 和 `bash -v` 选项的作用是什么？

> `-x`
> 
> 打印执行的命令及其参数。
> 
> `-v`
> 
> 打印读取的 shell 输入行。

**24)** `shellcheck` 是什么，你会在什么情况下使用它？

[shellcheck](https://www.shellcheck.net/) 是一个静态分析工具，它为脚本提供警告和建议。

根据 `man shellcheck`：

> ShellCheck 是用于 sh/bash 脚本的静态分析和 linting 工具。它主要关注处理典型的初学者和中级语法错误和陷阱，在这些情况下，shell 只会给出神秘的错误消息或奇怪的行为，但它也会报告一些更高级的问题，在这些情况下，边缘情况可能导致延迟失败。

## Shell Customization

**1)** 你会使用哪个命令来显示所有或特定环境变量的名称和值？

```sh
$ whatis printenv
printenv (1)         - print all or part of environment 
```

**2)** 如果你为已存在的命令（例如`ls`）添加了一个别名，你将如何调用原始命令而不是别名？

通过在前面加`\`或使用`command`内建命令。例如，`\ls`或`command ls`。

**3)** 为什么下面的别名不起作用？你会用什么呢？

你不能向别名传递参数，需要使用函数。

```sh
# doesn't work as expected
$ alias ext='echo "${1##*.}"'
$ ext ip.txt
 ip.txt

# expected output
$ ext() { echo "${1##*.}" ; }
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
$ unalias hw
$ hw
hw: command not found

$ hw() { echo hello there ; }
$ hw
hello there
$ unset -f hw
$ hw
hw: command not found 
```

**5)** 编写一个别名和一个函数，通过将`:`转换为换行符，在单独的行上显示`PATH`环境变量的内容。以下是一个示例输出。

```sh
$ echo "$PATH"
/usr/local/bin:/usr/bin:/bin:/usr/games

# alias
$ alias a_p='echo "$PATH" | tr ":" "\n"'
$ a_p
/usr/local/bin
/usr/bin
/bin
/usr/games

# function
$ f_p() { echo "${PATH//:/$'\n'}" ; }
$ f_p
/usr/local/bin
/usr/bin
/bin
/usr/games 
```

**6)** 登录 shell 是否会自动读取并执行`~/.bashrc`？

不。从`info bash`中得知：

> 当启动一个不是登录 shell 的交互式 shell 时，如果存在`~/.bashrc`文件，Bash 会读取并执行该文件中的命令。

参见[unix.stackexchange：为什么 bashrc 检查当前 shell 是否是交互式的？](https://unix.stackexchange.com/q/257571/109046)

**7)** 如果你希望有无限的历史条目，应该将`HISTSIZE`赋值为多少？

任何负数。

> `HISTSIZE`
> 
> 历史列表中需要记住的最大命令数。如果值为 0，则命令不会保存在历史列表中。数值小于 0 会导致每个命令都保存在历史列表中（没有限制）。在读取任何启动文件后，shell 将默认值设置为 500。

**8)** 绑定`set completion-ignore-case on`的作用是什么？

> `completion-ignore-case`
> 
> 如果设置为`on`，Readline 将以不区分大小写的方式执行文件名匹配和完成。默认值是`off`。

**9)** 哪个快捷键可以帮助你交互式地搜索命令历史？

> 要在历史中向后搜索特定字符串，请输入`C-r`。输入`C-s`则通过历史向前搜索。

**10)** 短路键`Alt+b`和`Alt+f`的作用是什么？

> `forward-word (M-f)`
> 
> 向前移动到下一个单词的末尾。单词由字母和数字组成。
> 
> `backward-word (M-b)`
> 
> 返回到当前或上一个单词的开头。单词由字母和数字组成。

**11)** `Ctrl+l`快捷键和`clear`命令之间有区别吗？

`Ctrl+l`保留已输入的内容，不会尝试完全删除回滚缓冲区。你可以使用`clear`命令来达到这个目的。

**12)** 你会使用哪个快捷键来删除光标前的字符直到行首？

> `unix-line-discard (C-u)`
> 
> 从光标处向后删除到当前行的开头。

**13)** 短路键`Alt+t`和`Ctrl+t`的作用是什么？

> `transpose-chars (C-t)`
> 
> 将光标前的字符向前拖动到光标处的字符上，同时将光标也向前移动。如果插入点位于行尾，则交换该行的最后两个字符。负数参数没有效果。
> 
> `transpose-words (M-t)`
> 
> 将点之前的单词拖过点，同时将点也移动过那个单词。如果插入点位于行尾，则交换该行最后两个单词。

**14)** `Shift+Insert` 和 `Shift+Ctrl+v` 快捷键之间有区别吗？

+   `Shift+Ctrl+v` 粘贴剪贴板内容

+   `Shift+Insert` 粘贴最后高亮的文本部分（不一定是剪贴板内容）
