# `expr` 命令

`expr` 命令评估给定的表达式并显示其相应的输出。它用于对整数执行基本的操作，如加法、减法、乘法、除法和取模，以及评估正则表达式，字符串操作，如子字符串、字符串长度等。

## 语法

```sh
      expr expression 

```

## 少量示例：

1.  ### 使用 expr 命令执行基本的算术运算

```sh
      expr 7 + 14
expr 7 * 8 

```

1.  ### 比较两个表达式

```sh
      x=10
y=20
res=`expr $x = $y`
echo $res 

```

1.  ### 匹配两个字符串中的字符数

```sh
      expr alphabet : alpha 

```

1.  ### 查找模数值

```sh
      expr 20 % 30 

```

1.  ### 提取子字符串

```sh
      a=HelloWorld
b=`expr substr $a 6 10`
echo $b 

```

### 其他标志及其功能

| **标志** | **描述** |
| :-- | :-- |
| `--version` | 输出版本信息并退出 |
| `--help` | 显示此帮助信息并退出 |

更多详情：[Expr 在维基百科上的介绍](https://en.wikipedia.org/wiki/Expr)
