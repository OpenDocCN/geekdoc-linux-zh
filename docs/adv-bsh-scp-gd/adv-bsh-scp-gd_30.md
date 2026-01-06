# 第二十五章\. 别名

> 原文：[`tldp.org/LDP/abs/html/aliases.html`](https://tldp.org/LDP/abs/html/aliases.html)

Bash *别名* 实质上不过是一个键盘快捷键，一个缩写，一种避免输入长命令序列的方法。例如，如果我们把 **alias lm="ls -l | more"** 包含在 `~/.bashrc` 文件`中，那么在命令行中输入的每个 `**lm**` [[1]](#FTN.AEN18669) 将会自动被替换为 **ls -l | more**。这可以在命令行中节省大量的输入，并避免记住复杂的命令和选项组合。设置 **alias rm="rm -i"**（交互模式删除）可能会节省很多麻烦，因为它可以防止意外删除重要文件。

在脚本中，别名的作用非常有限。如果别名能够承担一些 C 预处理器（如宏展开）的功能那就太好了，但不幸的是，Bash 不会在别名体内部展开参数。[[2]](#FTN.AEN18676) 此外，脚本本身在“复合结构”中（如 if/then 语句、循环和函数）也无法展开别名。另一个限制是别名不能递归展开。几乎总是，我们希望别名执行的操作都可以用 函数 来更有效地完成。

**示例 25-1\. 脚本中的别名**

```sh
#!/bin/bash
# alias.sh

shopt -s expand_aliases
# Must set this option, else script will not expand aliases.

# First, some fun.
alias Jesse_James='echo "\"Alias Jesse James\" was a 1959 comedy starring Bob Hope."'
Jesse_James

echo; echo; echo;

alias ll="ls -l"
# May use either single (') or double (") quotes to define an alias.

echo "Trying aliased \"ll\":"
ll /usr/X11R6/bin/mk*   #* Alias works.

echo

directory=/usr/X11R6/bin/
prefix=mk*  # See if wild card causes problems.
echo "Variables \"directory\" + \"prefix\" = $directory$prefix"
echo

alias lll="ls -l $directory$prefix"

echo "Trying aliased \"lll\":"
lll         # Long listing of all files in /usr/X11R6/bin stating with mk.
# An alias can handle concatenated variables -- including wild card -- o.k.

TRUE=1

echo

if [ TRUE ]
then
  alias rr="ls -l"
  echo "Trying aliased \"rr\" within if/then statement:"
  rr /usr/X11R6/bin/mk*   #* Error message results!
  # Aliases not expanded within compound statements.
  echo "However, previously expanded alias still recognized:"
  ll /usr/X11R6/bin/mk*
fi  

echo

count=0
while [ $count -lt 3 ]
do
  alias rrr="ls -l"
  echo "Trying aliased \"rrr\" within \"while\" loop:"
  rrr /usr/X11R6/bin/mk*   #* Alias will not expand here either.
                           #  alias.sh: line 57: rrr: command not found
  let count+=1
done 

echo; echo

alias xyz='cat $0'   # Script lists itself.
                     # Note strong quotes.
xyz
#  This seems to work,
#+ although the Bash documentation suggests that it shouldn't.
#
#  However, as Steve Jacobson points out,
#+ the "$0" parameter expands immediately upon declaration of the alias.

exit 0
```

**unalias** 命令用于删除之前设置的 *别名*。

**示例 25-2\. *unalias*：设置和取消别名**

```sh
#!/bin/bash
# unalias.sh

shopt -s expand_aliases  # Enables alias expansion.

alias llm='ls -al &#124; more'
llm

echo

unalias llm              # Unset alias.
llm
# Error message results, since 'llm' no longer recognized.

exit 0
```
```sh
bash$ **./unalias.sh**
total 6
drwxrwxr-x    2 bozo     bozo         3072 Feb  6 14:04 .
drwxr-xr-x   40 bozo     bozo         2048 Feb  6 14:04 ..
-rwxr-xr-x    1 bozo     bozo          199 Feb  6 14:04 unalias.sh

./unalias.sh: llm: command not found
```  |

### 注意事项

| [[1]](aliases.html#AEN18669) | ... 作为命令字符串的第一个单词。显然，别名仅在命令的 *开始* 处有意义。 |
| --- | --- |
| [[2]](aliases.html#AEN18676) | 然而，别名似乎可以展开位置参数。 |
