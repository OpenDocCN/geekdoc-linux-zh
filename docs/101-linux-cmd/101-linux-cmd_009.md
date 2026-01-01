# `bc` 命令

`bc` 命令提供了通过命令行执行数学计算的功能。

### 示例：

1. 算术：

```sh
      Input : $ echo "11+5" | bc
Output : 16 

```

2. 增量：

+   var –++ : 后增量运算符，首先使用变量的结果，然后增加变量。

+   – ++var : 预增量运算符，首先增加变量，然后存储变量的结果。

```sh
      Input: $ echo "var=3;++var" | bc
Output: 4 

```

3. 减量：

+   var – – : 后减量运算符，首先使用变量的结果，然后减少变量。

+   – – var : 预减量运算符，首先减少变量，然后存储变量的结果。

```sh
      Input: $ echo "var=3;--var" | bc
Output: 2 

```

4. 赋值：

+   var = value : 将值分配给变量

+   var += value : 类似于 var = var + value

+   var -= value : 类似于 var = var – value

+   var *= value : 类似于 var = var * value

+   var /= value : 类似于 var = var / value

+   var ^= value : 类似于 var = var ^ value

+   var %= value : 类似于 var = var % value

```sh
      Input: $ echo "var=4;var" | bc
Output: 4 

```

5. 比较或关系：

+   如果比较为真，则结果为 1。否则，（false），返回 0

+   expr1<expr2 : 如果 expr1 严格小于 expr2，则结果为 1。

+   expr1<=expr2 : 如果 expr1 小于或等于 expr2，则结果为 1。

+   expr1>expr2 : 如果 expr1 严格大于 expr2，则结果为 1。

+   expr1>=expr2 : 如果 expr1 大于或等于 expr2，则结果为 1。

+   expr1==expr2 : 如果 expr1 等于 expr2，则结果为 1。

+   expr1!=expr2 : 如果 expr1 不等于 expr2，则结果为 1。

```sh
      Input: $ echo "6<4" | bc
Output: 0 

```

```sh
      Input: $ echo "2==2" | bc
Output: 1 

```

6. 逻辑或布尔：

+   expr1 && expr2 : 如果两个表达式非零，则结果为 1。

+   expr1 || expr2 : 如果任一表达式非零，则结果为 1。

+   ! expr : 如果 expr 为 0，则结果为 1。

```sh
      Input: $ echo "! 1" | bc
Output: 0

Input: $ echo "10 && 5" | bc
Output: 1 

```

### 语法：

```sh
      bc [ -hlwsqv ] [long-options] [  file ... ] 

```

### 其他标志及其功能：

*注意：此列表并不全面。*

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-i` | `--interactive` | 强制交互模式 |
| `-l` | `--mathlib` | 使用预定义的数学例程 |
| `-q` | `--quiet` | 在不打印标题的情况下打开 `bc` 的交互模式 |
| `-s` | `--standard` | 将非标准 bc 构造视为错误 |
| `-w` | `--warn` | 如果使用非标准 bc 构造，则提供警告 |

### 注意：

1.  如果在脚本中使用，可以进一步欣赏 `bc` 的功能。除了基本的算术运算外，`bc` 还支持增量/减量、复杂计算、逻辑比较等。

1.  `bc` 中的两个标志涉及非标准构造。例如，如果你评估 `100>50 | bc`，你会得到一个奇怪的警告。根据 `bc` 的 POSIX 页面，关系运算符仅在用于 `if`、`while` 或 `for` 语句时有效。
