# Bash 数组

如果您曾经进行过任何编程，您可能已经熟悉数组了。

但以防您不是开发者，您需要知道的主要事情是，与变量不同，数组可以在一个名称下存储多个值。

您可以通过在 `()` 中用空格分隔的值来初始化数组。例如：

```sh
      my_array=("value 1" "value 2" "value 3" "value 4") 

```

要访问数组中的元素，您需要通过它们的数字索引来引用它们。

> **注意：**请记住，您需要使用花括号。

+   访问单个元素，这将输出：`值 2`

```sh
      echo ${my_array[1]} 

```

+   这将返回最后一个元素：`值 4`

```sh
      echo ${my_array[-1]} 

```

+   与使用 `@` 的命令行参数类似，这将返回数组中的所有元素，如下所示：`值 1 值 2 值 3 值 4`

```sh
      echo ${my_array[@]} 

```

+   在数组前加上哈希符号（`#`）将输出数组中的元素总数，在我们的例子中是 `4`：

```sh
      echo ${#my_array[@]} 

```

确保在您的端测试此功能并练习使用不同的值。

## 数组切片

虽然 Bash 不支持真正的数组切片，但您可以通过组合数组索引和字符串切片来实现类似的结果：

```sh
      #!/bin/bash 
array=("A" "B" "C" "D" "E")

# Print entire array
echo "${array[@]}"  # Output: A B C D E

# Access a single element
echo "${array[1]}"  # Output: B

# Print a range of elements (requires Bash 4.0+)
echo "${array[@]:1:3}"  # Output: B C D

# Print from an index to the end
echo "${array[@]:3}"  # Output: D E 

```

当处理数组时，始终使用 `[@]` 来引用所有元素，并将参数扩展放在引号内以保留数组元素中的空格。

## 字符串切片

在 Bash 中，您可以使用切片提取字符串的一部分。基本语法是：

```sh
      ${string:start:length} 

```

其中：

+   `start` 是起始索引（基于 0）

+   `length` 是要提取的最大字符数

让我们看看一些例子：

```sh
      #!/bin/bash 
text="ABCDE"

# Extract from index 0, maximum 2 characters
echo "${text:0:2}"  # Output: AB

# Extract from index 3 to the end
echo "${text:3}"    # Output: DE

# Extract 3 characters starting from index 1
echo "${text:1:3}"  # Output: BCD

# If length exceeds remaining characters, it stops at the end
echo "${text:3:3}"  # Output: DE (only 2 characters available) 

```

注意，切片表示法中的第二个数字表示提取子字符串的最大长度，而不是结束索引。这与一些其他编程语言（如 Python）不同。在 Bash 中，如果您指定的长度会超出字符串的末尾，它将简单地停止在字符串末尾而不会引发错误。

例如：

```sh
      text="Hello, World!"

# Extract 5 characters starting from index 7
echo "${text:7:5}"  # Output: World

# Attempt to extract 10 characters starting from index 7
# (even though only 6 characters remain)
echo "${text:7:10}"  # Output: World! 

```

在第二个例子中，尽管我们请求了 10 个字符，但 Bash 只返回从索引 7 到字符串末尾的 6 个可用字符。这种行为在您不确定正在处理的字符串的确切长度时尤其有用。
