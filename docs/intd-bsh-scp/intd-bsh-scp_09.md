# Bash 条件表达式

在计算机科学中，条件语句、条件表达式和条件构造是编程语言的特征，它们根据程序员指定的布尔条件是否评估为 true 或 false 来执行不同的计算或操作。

在 Bash 中，条件表达式通过 `[[` 复合命令和 `[` 内置命令用于测试文件属性和执行字符串和算术比较。

这里是 Bash 条件表达式中最受欢迎的列表。你不必死记硬背。你只需在需要时参考此列表即可！

## 文件表达式

+   如果文件存在，则返回 true。

```sh
      [[ -a ${file} ]] 

```

+   如果文件存在且是块特殊文件，则返回 true。

```sh
      [[ -b ${file} ]] 

```

+   如果文件存在且是字符特殊文件，则返回 true。

```sh
      [[ -c ${file} ]] 

```

+   如果文件存在且是目录，则返回 true。

```sh
      [[ -d ${file} ]] 

```

+   如果文件存在，则返回 true。

```sh
      [[ -e ${file} ]] 

```

+   如果文件存在且是常规文件，则返回 true。

```sh
      [[ -f ${file} ]] 

```

+   如果文件存在且是符号链接，则返回 true。

```sh
      [[ -h ${file} ]] 

```

+   如果文件存在且可读，则返回 true。

```sh
      [[ -r ${file} ]] 

```

+   如果文件存在且大小大于零，则返回 true。

```sh
      [[ -s ${file} ]] 

```

+   如果文件存在且可写，则返回 true。

```sh
      [[ -w ${file} ]] 

```

+   如果文件存在且可执行，则返回 true。

```sh
      [[ -x ${file} ]] 

```

+   如果文件存在且是符号链接，则返回 true。

```sh
      [[ -L ${file} ]] 

```

## 字符串表达式

+   如果 shell 变量 varname 已设置（已分配一个值），则返回 true。

```sh
      [[ -v varname ]] 

```

> 在这里，`varname` 是变量的名称。`-v` 运算符期望一个变量名作为参数而不是值，所以如果你传递 `${varname}` 而不是 `varname`，则表达式将返回 false。

如果字符串长度为零，则返回 true。

```sh
      [[ -z ${string} ]] 

```

如果字符串长度非零，则返回 true。

```sh
      [[ -n ${string} ]] 

```

+   如果字符串相等，则返回 true。`=` 应用于测试命令以符合 POSIX 标准。当与 `[[` 命令一起使用时，它执行上述描述的模式匹配（复合命令）。

```sh
      [[ ${string1} == ${string2} ]] 

```

+   如果字符串不相等，则返回 true。

```sh
      [[ ${string1} != ${string2} ]] 

```

+   如果 string1 在 lexicographically 排序上位于 string2 之前，则返回 true。

```sh
      [[ ${string1} < ${string2} ]] 

```

+   如果 string1 在 lexicographically 排序上位于 string2 之后，则返回 true。

```sh
      [[ ${string1} > ${string2} ]] 

```

## 算术运算符

+   如果数字相等，则返回 true

```sh
      [[ ${arg1} -eq ${arg2} ]] 

```

+   如果数字不相等，则返回 true

```sh
      [[ ${arg1} -ne ${arg2} ]] 

```

+   如果 arg1 小于 arg2，则返回 true

```sh
      [[ ${arg1} -lt ${arg2} ]] 

```

+   如果 arg1 小于或等于 arg2，则返回 true

```sh
      [[ ${arg1} -le ${arg2} ]] 

```

+   如果 arg1 大于 arg2，则返回 true

```sh
      [[ ${arg1} -gt ${arg2} ]] 

```

+   如果 arg1 大于或等于 arg2，则返回 true

```sh
      [[ ${arg1} -ge ${arg2} ]] 

```

作为旁注，arg1 和 arg2 可能是正数或负整数。

与其他编程语言一样，你可以使用 `AND` & `OR` 条件：

```sh
      [[ test_case_1 ]] && [[ test_case_2 ]] # And
[[ test_case_1 ]] || [[ test_case_2 ]] # Or 

```

## 退出状态运算符

+   如果命令成功执行且没有错误，则返回 true

```sh
      [[ $? -eq 0 ]] 

```

+   如果命令未成功执行或出现错误，则返回 true

```sh
      [[ $? -gt 0 ]] 

```
