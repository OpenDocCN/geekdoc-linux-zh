# 附录 J. 可编程完成简介

> 原文：[`tldp.org/LDP/abs/html/tabexpansion.html`](https://tldp.org/LDP/abs/html/tabexpansion.html)

Bash 中的 *可编程完成* 功能允许输入部分命令，然后按**[Tab]**键来自动完成命令序列。[[1]](#FTN.AEN24082) 如果有多个完成选项，则**[Tab]**键会列出所有选项。让我们看看它是如何工作的。

```sh
bash$ **xtra[Tab]**
xtraceroute       xtrapin           xtrapproto
 xtraceroute.real  xtrapinfo         xtrapreset
 xtrapchar         xtrapout          xtrapstats

bash$ **xtrac[Tab]**
xtraceroute       xtraceroute.real

bash$ **xtraceroute.r[Tab]**
xtraceroute.real

```

制表符完成也适用于变量和路径名。

```sh
bash$ **echo $BASH[Tab]**
$BASH                 $BASH_COMPLETION      $BASH_SUBSHELL
 $BASH_ARGC            $BASH_COMPLETION_DIR  $BASH_VERSINFO
 $BASH_ARGV            $BASH_LINENO          $BASH_VERSION
 $BASH_COMMAND         $BASH_SOURCE

bash$ **echo /usr/local/[Tab]**
bin/     etc/     include/ libexec/ sbin/    src/     
 doc/     games/   lib/     man/     share/

```

Bash 的 **complete** 和 **compgen** 内置命令 使得 *制表符完成* 能够识别命令的部分 *参数* 和 *选项*。在非常简单的情况下，我们可以使用命令行的 **complete** 来指定一组可接受的参数。

```sh
bash$ **touch sample_command**
bash$ **touch file1.txt file2.txt file2.doc file30.txt file4.zzz**
bash$ **chmod +x sample_command**
bash$ **complete -f -X '!*.txt' sample_command**

bash$ **./sample[Tab][Tab]**
sample_command
file1.txt   file2.txt   file30.txt

```

`-f` 选项用于 *complete* 指定文件名，而 `-X` 用于过滤模式。

对于更复杂的情况，我们可以编写一个脚本，指定一组可接受的命令行参数。**compgen** 内置命令将一组 *参数* 扩展为 *生成* 完成匹配。

让我们以 *UseGetOpt.sh* 脚本的 修改版本 作为示例命令。此脚本接受一系列命令行参数，这些参数前面可以是单个或双破折号。这里是对应的 *完成脚本*，按照惯例，文件名与相关命令相对应。

**示例 J-1\. 为 *UseGetOpt.sh* 编写的完成脚本***

```sh
# file: UseGetOpt-2
# UseGetOpt-2.sh parameter-completion

_UseGetOpt-2 ()   #  By convention, the function name
{                 #+ starts with an underscore.
  local cur
  # Pointer to current completion word.
  # By convention, it's named "cur" but this isn't strictly necessary.

  COMPREPLY=()   # Array variable storing the possible completions.
  cur=${COMP_WORDS[COMP_CWORD]}

  case "$cur" in
    -*)
    COMPREPLY=( $( compgen -W '-a -d -f -l -t -h --aoption --debug \
                               --file --log --test --help --' -- $cur ) );;
#   Generate the completion matches and load them into $COMPREPLY array.
#   xx) May add more cases here.
#   yy)
#   zz)
  esac

  return 0
}

complete -F _UseGetOpt-2 -o filenames ./UseGetOpt-2.sh
#        ^^ ^^^^^^^^^^^^  Invokes the function _UseGetOpt-2.
```

现在，让我们试试。

```sh
bash$ **source UseGetOpt-2**

bash$ **./UseGetOpt-2.sh -[Tab]**
--         --aoption  --debug    --file     --help     --log     --test
 -a         -d         -f         -h         -l         -t

bash$ **./UseGetOpt-2.sh --[Tab]**
--         --aoption  --debug    --file     --help     --log     --test

```

我们首先通过 source “完成脚本”。这设置了命令行参数。[[2]](#FTN.AEN24160)

首先，在单个破折号后按**[Tab]**键，输出的是所有可能的参数，前面带有一个或多个破折号。在两个破折号后按**[Tab]**键，则输出的是前面带有两个或更多破折号的参数。

现在，为什么要费尽周折去启用命令行的制表符完成功能呢？*它节省了按键次数。[[3]](#FTN.AEN24173)

--

*资源：*

Bash [可编程完成](http://freshmeat.net/projects/bashcompletion) 项目

米奇·弗雷泽的 [*Linux Journal*](http://www.linuxjournal.com) 文章，[*关于使用 Bash Complete 命令的更多内容*](http://www.linuxjournal.com/content/more-using-bash-complete-command)

史蒂夫出色的两篇系列文章，“Bash 完成功能简介”：[第一部分](http://www.debian-administration.org/article/An_introduction_to_bash_completion_part_1) 和 [第二部分](http://www.debian-administration.org/article/An_introduction_to_bash_completion_part_2)

### 注意事项

| [[1]](tabexpansion.html#AEN24082) | 这当然只适用于 *命令行*，而不适用于脚本内部。 |
| --- | --- |
| [[2]](tabexpansion.html#AEN24160) | 通常，默认参数补全文件位于 `/etc/profile.d` 目录或 `/etc/bash_completion` 目录中。这些文件在系统启动时自动加载。因此，在编写一个有用的补全脚本后，你可能希望将其（当然，作为 *root* 用户）移动到这些目录之一。 |
| [[3]](tabexpansion.html#AEN24173) | 已有大量文献记载，程序员愿意投入长时间的努力以节省十分钟“不必要的”劳动。这被称为 *优化*。 |
