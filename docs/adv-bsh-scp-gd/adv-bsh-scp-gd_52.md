# 附录 B. 参考卡

> 原文：[`tldp.org/LDP/abs/html/refcards.html`](https://tldp.org/LDP/abs/html/refcards.html)

以下参考卡提供了一些脚本概念的实用 *总结*。前面的文本更深入地讨论了这些问题，并给出了使用示例。

**表 B-1\. 特殊 Shell 变量**

| 变量 | 含义 |
| --- | --- |
| `$0` | 脚本文件名 |
| `$1` | 位置参数 #1 |
| `$2 - $9` | 位置参数 #2 - #9 |
| `${10}` | 位置参数 #10 |
| `$#` | 位置参数的数量 |
| `"$*"` | 所有位置参数（作为一个单词） * |
| `"$@"` | 所有位置参数（作为单独的字符串） |
| `${#*}` | 位置参数的数量 |
| `${#@}` | 位置参数的数量 |
| `$?` | 返回值 |
| `$$` | 脚本的进程 ID (PID) |
| `$-` | 传递给脚本的标志（使用 *set*） |
| `$_` | 上一个命令的最后一个参数 |
| `$!` | 最后在后台运行的作业的进程 ID (PID) |

***** *必须引用，否则默认为 `$@`。*

**表 B-2\. 测试操作符：二进制比较**

| 操作符 | 含义 | ----- | 操作符 | 含义 |
| --- | --- | --- | --- | --- |
|   |   |   |   |   |
| 算术比较 |   |   | 字符串比较 |   |
| `-eq` | 等于 |   | `=` | 等于 |
|   |   |   | `==` | 等于 |
| `-ne` | 不等于 |   | `!=` | 不等于 |
| `-lt` | 小于 |   | `\<` | 小于 (ASCII) * |
| `-le` | 小于或等于 |   |   |   |
| `-gt` | 大于 |   | `\>` | 大于 (ASCII) * |
| `-ge` | 大于或等于 |   |   |   |
|   |   |   | `-z` | 字符串为空 |
|   |   |   | `-n` | 字符串不为空 |
|   |   |   |   |   |
| 算术比较 | 在双括号内 (( ... )) |   |   |   |
| `>` | 大于 |   |   |   |
| `>=` | 大于或等于 |   |   |   |
| `<` | 小于 |   |   |   |
| `<=` | 小于或等于 |   |   |   |

***** *如果在一个双括号* [[ ... ]] *测试结构中，则不需要转义* \ *。*

**表 B-3\. 测试操作符：文件**

| 操作符 | 测试是否 | ----- | 操作符 | 测试是否 |
| --- | --- | --- | --- | --- |
| `-e` | 文件存在 |   | `-s` | 文件不是零大小 |
| `-f` | 文件是一个 *常规* 文件 |   |   |   |
| `-d` | 文件是一个 *目录* |   | `-r` | 文件有 *读* 权限 |
| `-h` | 文件是一个 符号链接 |   | `-w` | 文件有 *写* 权限 |
| `-L` | 文件是一个 *符号链接* |   | `-x` | 文件有 *执行* 权限 |
| `-b` | 文件是一个 块设备 |   |   |   |
| `-c` | 文件是一个 字符设备 |   | `-g` | *sgid* 标志设置 |
| `-p` | 文件是一个 管道 |   | `-u` | *suid* 标志设置 |
| `-S` | 文件是一个 套接字 |   | `-k` | 设置了 "粘性位" |
| `-t` | 文件与一个 *终端* 关联 |   |   |   |
|   |   |   |   |   |
| `-N` | 文件自上次读取以来已修改 |   | `F1 -nt F2` | 文件 F1 比 F2 *新* |
| `-O` | 你拥有该文件 |   | `F1 -ot F2` | 文件 F1 比 F2 *旧* |
| `-G` | 文件的 *组 id* 与你的相同 |   | `F1 -ef F2` | 文件 F1 和 F2 是指向同一文件的 *硬链接* * |
|   |   |   |   |   |
| `!` | NOT（反转上述测试的意义） |   |   |   |

***** 二元运算符（需要两个操作数）。

**表 B-4\. 参数替换和扩展**

| 表达式 | 含义 |
| --- | --- |
| `${var}` | `*var*` 的值（与 `*$var*` 相同） |
|   |   |
| `${var-$DEFAULT}` | 如果 `*var*` 未设置，评估 表达式为 `*$DEFAULT*` * |
| `${var:-$DEFAULT}` | 如果 `*var*` 未设置或为空，则 *评估* 表达式为 `*$DEFAULT*` * |
|   |   |
| `${var=$DEFAULT}` | 如果 `*var*` 未设置，则将表达式评估为 `*$DEFAULT*` * |
| `${var:=$DEFAULT}` | 如果 `*var*` 未设置或为空，则将表达式评估为 `*$DEFAULT*` * |
|   |   |
| `${var+$OTHER}` | 如果 `*var*` 已设置，则将表达式评估为 `*$OTHER*`，否则为空字符串 |
| `${var:+$OTHER}` | 如果 `*var*` 已设置，则将表达式评估为 `*$OTHER*`，否则为空字符串 |
|   |   |
| `${var?$ERR_MSG}` | 如果 `*var*` 未设置，打印 `*$ERR_MSG*` 并以退出状态 1 中断脚本。* |
| `${var:?$ERR_MSG}` | 如果 `*var*` 未设置，打印 `*$ERR_MSG*` 并以退出状态 1 中断脚本。* |
|   |   |
| `${!varprefix*}` | 匹配所有以前声明的以 `*varprefix*` 开头的变量 |
| `${!varprefix@}` | 匹配所有以前声明的以 `*varprefix*` 开头的变量 |

***** 如果 `*var*` *已*设置，则评估表达式为 `*$var*`，没有副作用。

**# 注意**，上述运算符的一些行为与 Bash 的早期版本有所不同。

**表 B-5\. 字符串操作**

| 表达式 | 含义 |
| --- | --- |
| `${#string}` | `*$string*` 的长度 |
|   |   |
| `${string:position}` | 从 `*$string*` 中提取 `*$position*` 位置的子字符串 |
| `${string:position:length}` | 从 `*$string*` 的 `*$position*` 位置提取 `*$length*` 个字符的子字符串 [零索引，第一个字符位于位置 0] |
|   |   |
| `${string#substring}` | 从 `*$string*` 的前面移除最短的 `*$substring*` 匹配项 |
| `${string##substring}` | 从 `*$string*` 的前面移除最长的 `*$substring*` 匹配项 |
| `${string%substring}` | 从 `*$string*` 的后面移除最短的 `*$substring*` 匹配项 |
| `${string%%substring}` | 从 `*$string*` 的后面移除最长的 `*$substring*` 匹配项 |
|   |   |
| `${string/substring/replacement}` | 将 `*$substring*` 的第一个匹配项替换为 `*$replacement*` |
| `${string//substring/replacement}` | 将 `*$substring*` 的所有匹配项替换为 `*$replacement*` |
| `${string/#substring/replacement}` | 如果 `*$substring*` 匹配 `*$string*` 的 *开头*，则用 `*$replacement*` 替换 `*$substring*` |
| `${string/%substring/replacement}` | 如果 `*$substring*` 匹配 `*$string*` 的 *末尾*，则用 `*$replacement*` 替换 `*$substring*` |
|   |   |
|   |   |
| `expr match "$string" '$substring'` | 在 `*$string*` 开头匹配的 `*$substring*` 的长度 |
| `expr "$string" : '$substring'` | 在 `*$string*` 开头匹配的 `*$substring*` 的长度 |
| `expr index "$string" $substring` | `*$string*` 中匹配 `*$substring*` 的第一个字符的数值位置 [如果没有匹配，则位置为 0，第一个字符计为位置 1] |
| `expr substr $string $position $length` | 从 `*$string*` 的 `*$position*` 开始提取 `*$length*` 个字符 [如果没有匹配，则位置为 0，第一个字符计为位置 1] |
| `expr match "$string" '\($substring\)'` | 提取 `*$substring*`，从 `*$string*` 的开头搜索 |
| `expr "$string" : '\($substring\)'` | 提取 `*$substring*`，从 `*$string*` 的开头搜索 |
| `expr match "$string" '.*\($substring\)'` | 从 `*$string*` 的末尾提取 `*$substring*` |
| `expr "$string" : '.*\($substring\)'` | 从 `*$string*` 的末尾提取 `*$substring*` |

***** 其中 `*$substring*` 是一个 正则表达式。

**表 B-6. 其他构造**

| 表达式 | 解释 |
| --- | --- |
|   |   |
| 方括号 |   |
| `if [ CONDITION ]` | 测试构造 |
| `if [[ CONDITION ]]` | 扩展测试构造 |
| `Array[1]=element1` | 数组初始化 |
| `[a-z]` | 正则表达式 中 字符范围 |
|   |   |
| 大括号 |   |
| `${variable}` | 参数替换 |
| `${!variable}` | 间接变量引用 |
| `{ command1; command2; . . . commandN; }` | 代码块 |
| `{string1,string2,string3,...}` | 大括号展开 |
| `{a..z}` | 扩展大括号展开 |
| `{}` | 文本替换，在 find 和 xargs 之后 |
|   |   |
|   |   |
| 圆括号 |   |
| `( command1; command2 )` | 在 子 shell 中执行的命令组 |
| `Array=(element1 element2 element3)` | 数组初始化 |
| `result=$(COMMAND)` | 命令替换，新样式 |
| `>(COMMAND)` | 进程替换 |
| `<(COMMAND)` | 进程替换 |
|   |   |
| 双圆括号 |   |
| `(( var = 78 ))` | 整数算术 |
| `var=$(( 20 + 5 ))` | 整数运算，带有变量赋值 |
| `(( var++ ))` | *C 风格* 变量递增 |
| `(( var-- ))` | *C 风格* 变量递减 |
| `(( var0 = var1<98?9:21 ))` | *C 风格* 三元操作 |
|   |   |
| 引用 |   |
| `"$variable"` | "弱引用" |
| `'string'` | '强引用' |
|   |   |
| 反引号 |   |
| `` result=`COMMAND` `` | 命令替换，经典风格 |
