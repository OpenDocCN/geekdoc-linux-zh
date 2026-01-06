# 第十三章\. 算术扩展

> 原文：[`tldp.org/LDP/abs/html/arithexp.html`](https://tldp.org/LDP/abs/html/arithexp.html)

算术扩展为在脚本中执行（整数）算术运算提供了一个强大的工具。使用 *反引号*、*双括号* 或 *let* 将字符串转换为数值表达式相对直接。

**变体**

使用 反引号（通常与 expr 一起使用）进行算术扩展

```sh
z=`expr $z + 3`          # The 'expr' command performs the expansion.
```

使用 双括号 进行算术扩展，并使用 let

在算术扩展中使用 *反引号*（*backquotes*）已被 *双括号* -- `**((...))**` 和 `**$((...))**` -- 以及非常方便的 let 构造所取代。

```sh
z=$(($z+3))
z=$((z+3))                                  #  Also correct.
                                            #  Within double parentheses,
                                            #+ parameter dereferencing
                                            #+ is optional.

# $((EXPRESSION)) is arithmetic expansion.  #  Not to be confused with
                                            #+ command substitution.

# You may also use operations within double parentheses without assignment.

  n=0
  echo "n = $n"                             # n = 0

  (( n += 1 ))                              # Increment.
# (( $n += 1 )) is incorrect!
  echo "n = $n"                             # n = 1

let z=z+3
let "z += 3"  #  Quotes permit the use of spaces in variable assignment.
              #  The 'let' operator actually performs arithmetic evaluation,
              #+ rather than expansion.
```

脚本中算术扩展的示例：

1.  示例 16-9

1.  示例 11-15

1.  示例 27-1

1.  示例 27-11

1.  示例 A-16
