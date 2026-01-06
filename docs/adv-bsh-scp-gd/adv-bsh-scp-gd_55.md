# C.2\. Awk

> 原文：[`tldp.org/LDP/abs/html/awk.html`](https://tldp.org/LDP/abs/html/awk.html)

*Awk* [[1]](#FTN.AEN23443) 是一种功能齐全的文本处理语言，其语法类似于 *C*。虽然它具有广泛的操作符和功能，但在这里我们将只介绍其中的一些——在 shell 脚本中最有用的那些。

Awk 将其接收到的每一行输入分解为字段。默认情况下，字段是由空白字符分隔的连续字符字符串，尽管有选项可以更改这一点。Awk 解析并操作每个单独的字段。这使得它非常适合处理结构化文本文件——特别是表格——数据组织成一致的块，如行和列。

强引用和花括号将 awk 代码块包围在 shell 脚本中。

```sh
# $1 is field #1, $2 is field #2, etc.

echo one two &#124; awk '{print $1}'
# one

echo one two &#124; awk '{print $2}'
# two

# But what is field #0 ($0)?
echo one two &#124; awk '{print $0}'
# one two
# All the fields!

awk '{print $3}' $filename
# Prints field #3 of file $filename to stdout.

awk '{print $1 $5 $6}' $filename
# Prints fields #1, #5, and #6 of file $filename.

awk '{print $0}' $filename
# Prints the entire file!
# Same effect as:   cat $filename . . . or . . . sed '' $filename
```

我们刚刚看到了 awk *print*命令的实际应用。awk 在这里需要处理的另一个功能是变量。awk 处理变量的方式与 shell 脚本类似，但更加灵活。

```sh
{ total += ${column_number} }
```

这会将`*column_number*`的值添加到`*total*`的累计总和中。最后，为了打印"total"，有一个**END**命令块，在脚本处理完所有输入后执行。

```sh
END { print total }
```

与**END**相对应的是**BEGIN**，用于在 awk 开始处理输入之前执行代码块。

以下示例说明了**awk**如何将文本解析工具添加到 shell 脚本中。

**示例 C-1\. 计数字母出现次数**

```sh
#! /bin/sh
# letter-count2.sh: Counting letter occurrences in a text file.
#
# Script by nyal [nyal@voila.fr].
# Used in ABS Guide with permission.
# Recommented and reformatted by ABS Guide author.
# Version 1.1: Modified to work with gawk 3.1.3.
#              (Will still work with earlier versions.)

INIT_TAB_AWK=""
# Parameter to initialize awk script.
count_case=0
FILE_PARSE=$1

E_PARAMERR=85

usage()
{
    echo "Usage: letter-count.sh file letters" 2>&1
    # For example:   ./letter-count2.sh filename.txt a b c
    exit $E_PARAMERR  # Too few arguments passed to script.
}

if [ ! -f "$1" ] ; then
    echo "$1: No such file." 2>&1
    usage                 # Print usage message and exit.
fi 

if [ -z "$2" ] ; then
    echo "$2: No letters specified." 2>&1
    usage
fi 

shift                      # Letters specified.
for letter in `echo $@`    # For each one . . .
  do
  INIT_TAB_AWK="$INIT_TAB_AWK tab_search[${count_case}] = \
  \"$letter\"; final_tab[${count_case}] = 0; " 
  # Pass as parameter to awk script below.
  count_case=`expr $count_case + 1`
done

# DEBUG:
# echo $INIT_TAB_AWK;

cat $FILE_PARSE &#124;
# Pipe the target file to the following awk script.

# ---------------------------------------------------------------------
# Earlier version of script:
# awk -v tab_search=0 -v final_tab=0 -v tab=0 -v \
# nb_letter=0 -v chara=0 -v chara2=0 \

awk \
"BEGIN { $INIT_TAB_AWK } \
{ split(\$0, tab, \"\"); \
for (chara in tab) \
{ for (chara2 in tab_search) \
{ if (tab_search[chara2] == tab[chara]) { final_tab[chara2]++ } } } } \
END { for (chara in final_tab) \
{ print tab_search[chara] \" => \" final_tab[chara] } }"
# ---------------------------------------------------------------------
#  Nothing all that complicated, just . . .
#+ for-loops, if-tests, and a couple of specialized functions.

exit $?

# Compare this script to letter-count.sh.
```

要查看 shell 脚本中 awk 的更简单示例，请参阅：

1.  示例 15-14

1.  示例 20-8

1.  示例 16-32

1.  示例 36-5

1.  示例 28-2

1.  示例 15-20

1.  示例 29-3

1.  示例 29-4

1.  示例 11-3

1.  示例 16-61

1.  示例 9-16

1.  示例 16-4

1.  示例 10-6

1.  示例 36-19

1.  示例 11-9

1.  示例 36-4

1.  示例 16-53

1.  示例 T-3

好了，这就是我们要在这里介绍的 awk，但还有很多东西要学习。请参阅参考文献中的适当参考。

### 注意

| [[1]](awk.html#AEN23443) | 它的名字来源于其作者的缩写**A**ho, **W**einberg, 和 **K**ernighan。 |
| --- | --- |
