# 第三十四章\. 注意事项

> 原文：[`tldp.org/LDP/abs/html/gotchas.html`](https://tldp.org/LDP/abs/html/gotchas.html)

|   |  **Turandot: *谜题有三个，一个关乎死亡！**

*Caleph: *不，不！谜题有三个，一个关乎生命！**

*--Puccini**  |

这里有一些（非推荐！）脚本编写实践，可以让原本单调的生活变得有趣。

+   将保留词或字符分配给变量名。

    ```sh
    case=value0       # Causes problems.
    23skidoo=value1   # Also problems.
    # Variable names starting with a digit are reserved by the shell.
    # Try _23skidoo=value1\. Starting variables with an underscore is okay.

    # However . . .   using just an underscore will not work.
    _=25
    echo $_           # $_ is a special variable set to last arg of last command.
    # But . . .       _ is a valid function name!

    xyz((!*=value2    # Causes severe problems.
    # As of version 3 of Bash, periods are not allowed within variable names.
    ```

+   在变量名（或函数名）中使用连字符或其他保留字符。

    ```sh
    var-1=23
    # Use 'var_1' instead.

    function-whatever ()   # Error
    # Use 'function_whatever ()' instead.

    # As of version 3 of Bash, periods are not allowed within function names.
    function.whatever ()   # Error
    # Use 'functionWhatever ()' instead.
    ```

+   为变量和函数使用相同的名称。这可能会使脚本难以理解。

    ```sh
    do_something ()
    {
      echo "This function does something with \"$1\"."
    }

    do_something=do_something

    do_something do_something

    # All this is legal, but highly confusing.
    ```

+   不恰当地使用 空白字符。与其它编程语言相比，Bash 对空白字符相当挑剔。

    ```sh
    var1 = 23   # 'var1=23' is correct.
    # On line above, Bash attempts to execute command "var1"
    # with the arguments "=" and "23".

    let c = $a - $b   # Instead:   let c=$a-$b   or   let "c = $a - $b"

    if [ $a -le 5]    # if [ $a -le 5 ]   is correct.
    #           ^^      if [ "$a" -le 5 ]   is even better.
                      # [[ $a -le 5 ]] also works.
    ```

+   在花括号内的 代码块 的最后一个命令后没有使用 分号 结尾。

    ```sh
    { ls -l; df; echo "Done." }
    # bash: syntax error: unexpected end of file

    { ls -l; df; echo "Done."; }
    #                        ^     ### Final command needs semicolon.
    ```

+   假设未初始化的变量（在给它们赋值之前）是“置零”的。未初始化的变量具有 *null* 值，而不是零。

    ```sh
    #!/bin/bash

    echo "uninitialized_var = $uninitialized_var"
    # uninitialized_var =

    # However . . .
    # if $BASH_VERSION ≥ 4.2; then

    if [[ ! -v uninitialized_var ]]
    then
      uninitialized_var=0   # Initialize it to zero!
    fi

    ```

+   在测试中混淆了 *=* 和 *-eq*。记住，*=* 用于比较字面变量，而 *-eq* 用于整数。

    ```sh
    if [ "$a" = 273 ]      # Is $a an integer or string?
    if [ "$a" -eq 273 ]    # If $a is an integer.

    # Sometimes you can interchange -eq and = without adverse consequences.
    # However . . .

    a=273.0   # Not an integer.

    if [ "$a" = 273 ]
    then
      echo "Comparison works."
    else  
      echo "Comparison does not work."
    fi    # Comparison does not work.

    # Same with   a=" 273"  and a="0273".

    # Likewise, problems trying to use "-eq" with non-integer values.

    if [ "$a" -eq 273.0 ]
    then
      echo "a = $a"
    fi  # Aborts with an error message.  
    # test.sh: : 273.0: integer expression expected
    ```

+   错误地使用 [字符串比较 操作符。

    **示例 34-1\. 数字和字符串比较不等价**

    ```sh
    #!/bin/bash
    # bad-op.sh: Trying to use a string comparison on integers.

    echo
    number=1

    #  The following while-loop has two errors:
    #+ one blatant, and the other subtle.

    while [ "$number" < 5 ]    # Wrong! Should be:  while [ "$number" -lt 5 ]
    do
      echo -n "$number "
      let "number += 1"
    done  
    #  Attempt to run this bombs with the error message:
    #+ bad-op.sh: line 10: 5: No such file or directory
    #  Within single brackets, "<" must be escaped,
    #+ and even then, it's still wrong for comparing integers.

    echo "---------------------"

    while [ "$number" \< 5 ]    #  1 2 3 4
    do                          #
      echo -n "$number "        #  It *seems* to work, but . . .
      let "number += 1"         #+ it actually does an ASCII comparison,
    done                        #+ rather than a numerical one.

    echo; echo "---------------------"

    # This can cause problems. For example:

    lesser=5
    greater=105

    if [ "$greater" \< "$lesser" ]
    then
      echo "$greater is less than $lesser"
    fi                          # 105 is less than 5
    #  In fact, "105" actually is less than "5"
    #+ in a string comparison (ASCII sort order).

    echo

    exit 0
    ```

+   尝试使用 let 来设置字符串变量。

    ```sh
    let "a = hello, you"
    echo "$a"   # 0
    ```

+   有时在 "test" 括号 ([ ]) 内的变量需要引用（双引号）。不这样做可能会导致意外的行为。参见 示例 7-6，示例 20-5，和 示例 9-6。

+   引用包含空白的变量 防止分割。有时这会产生 意外的后果。

+   从脚本中发出的命令可能无法执行，因为脚本所有者没有对这些命令的执行权限。如果用户无法从命令行调用命令，那么将其放入脚本中也会同样失败。尝试更改相关命令的属性，也许甚至设置 suid 位（当然，作为 *root*）。

+   尝试使用 **-** 作为重定向操作符（它并不是）通常会带来不愉快的惊喜。

    ```sh
    command1 2> - &#124; command2
    # Trying to redirect error output of command1 into a pipe . . .
    # . . . will not work.	

    command1 2>& - &#124; command2  # Also futile.

    Thanks, S.C.
    ```

+   使用 Bash 版本 2+ 功能可能会导致错误信息下的退出。较老的 Linux 机器可能默认安装了 Bash 的 1.XX 版本。

    ```sh
    #!/bin/bash

    minimum_version=2
    # Since Chet Ramey is constantly adding features to Bash,
    # you may set $minimum_version to 2.XX, 3.XX, or whatever is appropriate.
    E_BAD_VERSION=80

    if [ "$BASH_VERSION" \< "$minimum_version" ]
    then
      echo "This script works only with Bash, version $minimum or greater."
      echo "Upgrade strongly recommended."
      exit $E_BAD_VERSION
    fi

    ...
    ```

+   在非 Linux 机器上的 Bourne shell 脚本（`**#!/bin/sh**`）中使用 Bash 特定功能 可能会导致意外的行为。Linux 系统通常将 **sh** 别名为 **bash**，但这并不一定适用于通用的 UNIX 机器。

+   在 Bash 中使用未记录的特性是一种危险的做法。在本书的先前版本中，有几个脚本依赖于“特性”，即尽管退出或返回值的最大值为 255，但该限制不适用于 *负数*。不幸的是，在 2.05b 及以后的版本中，这个漏洞消失了。参见 示例 24-9。

+   在某些上下文中，可能会返回一个误导性的 退出状态。这可能会发生在在函数中 设置局部变量 或将算术值分配给变量时（内部）。

+   一个算术表达式的 退出状态 并不等于一个 *错误代码*。

    ```sh
    var=1 && ((--var)) && echo $var
    #        ^^^^^^^^^ Here the and-list terminates with exit status 1.
    #                     $var doesn't echo!
    echo $?   # 1
    ```

+   使用 DOS 类型的换行符 (`*\r\n*`) 的脚本将无法执行，因为 `**#!/bin/bash\r\n**` 不会被识别，*不是*预期的 `**#!/bin/bash\n**`。修复方法是转换脚本为 UNIX 风格的换行符。

    ```sh
    #!/bin/bash

    echo "Here"

    unix2dos $0    # Script changes itself to DOS format.
    chmod 755 $0   # Change back to execute permission.
                   # The 'unix2dos' command removes execute permission.

    ./$0           # Script tries to run itself again.
                   # But it won't work as a DOS file.

    echo "There"

    exit 0
    ```

+   以 `**#!/bin/sh**` 开头的 shell 脚本不会在完全的 Bash 兼容模式下运行。一些特定的 Bash 函数可能会被禁用。需要完全访问所有 Bash 特定扩展的脚本应该以 `**#!/bin/bash**` 开始。

+   在 here document 的 终止限制字符串 前添加空格将导致脚本中出现意外的行为。

+   在一个 输出被捕获 的函数中放入多个 *echo* 语句。

    ```sh
    add2 ()
    {
      echo "Whatever ... "   # Delete this line!
      let "retval = $1 + $2"
        echo $retval
        }

        num1=12
        num2=43
        echo "Sum of $num1 and $num2 = $(add2 $num1 $num2)"

    #   Sum of 12 and 43 = Whatever ... 
    #   55

    #        The "echoes" concatenate.
    ```

    这将不会工作。

+   脚本可能无法将变量 **export** 回其 父进程、shell 或环境。正如我们在生物学中学到的，子进程可以继承自父进程，但反之则不然。

    ```sh
    WHATEVER=/home/bozo
    export WHATEVER
    exit 0
    ```
    ```sh
    bash$ echo $WHATEVER
     `bash$` 
    ```

    当然，回到命令提示符，$WHATEVER 仍然没有被设置。

+   在 子 shell 中设置和操作变量，然后尝试在子 shell 的作用域之外使用这些相同的变量，这会导致一个不愉快的惊喜。

    **示例 34-2\. 子 shell 的陷阱**

    ```sh
    #!/bin/bash
    # Pitfalls of variables in a subshell.

    outer_variable=outer
    echo
    echo "outer_variable = $outer_variable"
    echo

    (
    # Begin subshell

    echo "outer_variable inside subshell = $outer_variable"
    inner_variable=inner  # Set
    echo "inner_variable inside subshell = $inner_variable"
    outer_variable=inner  # Will value change globally?
    echo "outer_variable inside subshell = $outer_variable"

    # Will 'exporting' make a difference?
    #    export inner_variable
    #    export outer_variable
    # Try it and see.

    # End subshell
    )

    echo
    echo "inner_variable outside subshell = $inner_variable"  # Unset.
    echo "outer_variable outside subshell = $outer_variable"  # Unchanged.
    echo

    exit 0

    # What happens if you uncomment lines 19 and 20?
    # Does it make a difference?
    ```

+   将 **echo** 输出通过 read 管道传输可能会产生意外的结果。在这种情况下，**read** 行为就像它在子 shell 中运行一样。相反，使用 set 命令（如 示例 15-18 中所示）。

    **示例 34-3\. 将 *echo* 的输出通过管道传输到 *read***

    ```sh
    #!/bin/bash
    #  badread.sh:
    #  Attempting to use 'echo and 'read'
    #+ to assign variables non-interactively.

    #   shopt -s lastpipe

    a=aaa
    b=bbb
    c=ccc

    echo "one two three" &#124; read a b c
    # Try to reassign a, b, and c.

    echo
    echo "a = $a"  # a = aaa
    echo "b = $b"  # b = bbb
    echo "c = $c"  # c = ccc
    # Reassignment failed.

    ### However . . .
    ##  Uncommenting line 6:
    #   shopt -s lastpipe
    ##+ fixes the problem!
    ### This is a new feature in Bash, version 4.2.

    # ------------------------------

    # Try the following alternative.

    var=`echo "one two three"`
    set -- $var
    a=$1; b=$2; c=$3

    echo "-------"
    echo "a = $a"  # a = one
    echo "b = $b"  # b = two
    echo "c = $c"  # c = three 
    # Reassignment succeeded.

    # ------------------------------

    #  Note also that an echo to a 'read' works within a subshell.
    #  However, the value of the variable changes *only* within the subshell.

    a=aaa          # Starting all over again.
    b=bbb
    c=ccc

    echo; echo
    echo "one two three" &#124; ( read a b c;
    echo "Inside subshell: "; echo "a = $a"; echo "b = $b"; echo "c = $c" )
    # a = one
    # b = two
    # c = three
    echo "-----------------"
    echo "Outside subshell: "
    echo "a = $a"  # a = aaa
    echo "b = $b"  # b = bbb
    echo "c = $c"  # c = ccc
    echo

    exit 0
    ```

    实际上，正如安东尼·理查德森所指出的，将输出通过管道传输到 *任何* 循环都可能导致类似的问题。

    ```sh
    # Loop piping troubles.
    #  This example by Anthony Richardson,
    #+ with addendum by Wilbert Berendsen.

    foundone=false
    find $HOME -type f -atime +30 -size 100k &#124;
    while true
    do
       read f
       echo "$f is over 100KB and has not been accessed in over 30 days"
       echo "Consider moving the file to archives."
       foundone=true
       # ------------------------------------
         echo "Subshell level = $BASH_SUBSHELL"
       # Subshell level = 1
       # Yes, we're inside a subshell.
       # ------------------------------------
    done

    #  foundone will always be false here since it is
    #+ set to true inside a subshell
    if [ $foundone = false ]
    then
       echo "No files need archiving."
    fi

    # =====================Now, here is the correct way:=================

    foundone=false
    for f in $(find $HOME -type f -atime +30 -size 100k)  # No pipe here.
    do
       echo "$f is over 100KB and has not been accessed in over 30 days"
       echo "Consider moving the file to archives."
       foundone=true
    done

    if [ $foundone = false ]
    then
       echo "No files need archiving."
    fi

    # ==================And here is another alternative==================

    #  Places the part of the script that reads the variables
    #+ within a code block, so they share the same subshell.
    #  Thank you, W.B.

    find $HOME -type f -atime +30 -size 100k &#124; {
         foundone=false
         while read f
         do
           echo "$f is over 100KB and has not been accessed in over 30 days"
           echo "Consider moving the file to archives."
           foundone=true
         done

         if ! $foundone
         then
           echo "No files need archiving."
         fi
    }
    ```

    当尝试将 `stdout` 的 `tail -f` 管道输出到 grep 时，会出现类似的问题。

    ```sh
    tail -f /var/log/messages &#124; grep "$ERROR_MSG" >> error.log
    #  The "error.log" file will not have anything written to it.
    #  As Samuli Kaipiainen points out, this results from grep
    #+ buffering its output.
    #  The fix is to add the "--line-buffered" parameter to grep.
    ```

+   在脚本中使用“suid”命令是危险的，因为它可能会损害系统安全。1

+   使用 shell 脚本进行 CGI 编程可能存在问题。Shell 脚本变量不是“类型安全的”，这可能导致 CGI 出现不希望的行为。此外，很难使 shell 脚本“防破解”。

+   Bash 无法正确处理双斜杠(//)字符串。

+   为 Linux 或 BSD 系统编写的 Bash 脚本可能需要在商业 UNIX 机器上运行时进行修复。这类脚本通常使用 GNU 命令集和过滤器，它们的功能比通用的 UNIX 对应工具更强大。这尤其适用于像 tr 这样的文本处理工具。

+   悲哀的是，Bash 本身的更新破坏了以前运行良好的旧脚本曾经完美运行。让我们回忆一下使用未经记录的 Bash 功能的风险有多大。

|   |  **危险近在咫尺--*

*务必小心，务必小心，务必小心，务必小心。*

*许多勇敢的心已经沉睡在深渊之中。*

*所以务必小心--*

*务必小心。*

*--A.J. Lamb 和 H.W. Petrie**  |

### 注意事项

| [[1]](gotchas.html#AEN19993) | 在脚本本身上设置 suid 权限在 Linux 和大多数其他 UNIX 版本中没有任何效果。 |
| --- | --- |
