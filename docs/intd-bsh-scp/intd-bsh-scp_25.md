# 常见 Bash 错误和修复

Bash 脚本在 Linux 自动化中广泛使用，但初学者经常遇到可能令人困惑的错误。本章解释了常见的 Bash 错误、它们发生的原因以及如何修复它们。每个说明都包括示例，以便于理解和在实际场景中应用。

通过学习这些常见错误，您可以更快地调试脚本，编写更干净的代码，并避免重复错误。

## 1. 权限被拒绝

### 错误：

```sh
      bash: ./script.sh: Permission denied 

```

### 原因：

脚本文件不可执行，这阻止了 shell 运行它。

### 修复：

使用以下方法授予执行权限：

```sh
      chmod +x script.sh 

```

然后运行脚本：

```sh
      ./script.sh 

```

### 示例：

```sh
      #!/bin/bashecho "Hello, world!" 

```

没有使用`chmod +x`，运行脚本会导致错误。在授予执行权限后，脚本可以成功运行。

**说明：** Linux 出于安全原因需要显式权限才能运行脚本。始终在运行新脚本之前设置执行权限。

## 2. 错误的解释器或文件不存在

### 错误：

```sh
      bash: ./script.sh: bad interpreter: /bin/bash^M: No such file or directory 

```

### 原因：

这发生在脚本在 Windows 上创建时。Windows 使用回车换行符（`\r\n`），Linux 无法读取。

### 修复：

将文件转换为 Unix 格式：

```sh
      dos2unix script.sh 

```

或者使用 Linux 文本编辑器（如**nano**或**vim**）重新创建文件。

**说明：** 脚本的第一行（`#!/bin/bash`）告诉 shell 使用哪个解释器。额外的 Windows 字符会破坏这一行，导致错误。

## 3. 命令未找到

### 错误：

```sh
      bash: myscript: command not found 

```

### 原因：

shell 无法定位命令或脚本。它可能不在系统 PATH 中，或者未指定路径执行。

### 修复：

从其当前目录运行脚本：

```sh
      ./myscript.sh 

```

或者将其目录添加到 PATH 中：

```sh
      export PATH=$PATH:/path/to/script 

```

**说明：** shell 在`$PATH`中列出的目录中搜索命令。这些目录之外的脚本需要显式路径。

## 4. 在意外标记附近出现语法错误

### 错误：

```sh
      syntax error near unexpected token `then' 

```

### 原因：

缺少`then`，分号（`;`）或控制结构不正确会导致 Bash 无法解析脚本。

### 修复：

确保条件语句的语法正确。

**不正确：**

```sh
      if [ $num -gt 10 ]
echo "Number greater than 10"
fi 

```

**正确：**

```sh
      if [ $num -gt 10 ]; then
  echo "Number greater than 10"
fi 

```

**说明：** Bash 要求`if`语句具有特定的结构：`[条件]; then`后跟命令，并以`fi`结束。

## 5. 预期文件结束（EOF）

### 错误：

```sh
      syntax error: unexpected end of file 

```

### 原因：

块（如`if`、`for`或`while`）或引号未正确关闭。

### 示例：

**不正确：**

```sh
      if [ $num -gt 5 ]; then
  echo "Greater" 

```

**正确：**

```sh
      if [ $num -gt 5 ]; then
  echo "Greater"
fi 

```

**说明：** 每个开关键词（如`if`或`for`）都必须有一个相应的关闭关键词（`fi, done`），以便 Bash 理解代码块。

## 6. 变量未展开

## 问题：

变量打印空或未显示其值。

### 原因：

变量未初始化或未正确导出。

### 修复：

```sh
      username="Nishika"
echo "User is $username" 

```

**说明：** 变量在使用之前必须赋值。否则，Bash 不会打印任何内容。

## 7. 预期整数表达式

### 错误：

```sh
      [: 5a: integer expression expected 

```

### 原因：

在算术比较中使用非数值值。

### 修复：

```sh
      num=5
if [ $num -gt 3 ]; then
  echo "Yes"
fi 

```

**说明：** Bash 的算术比较只适用于整数。请确保变量包含一个数字。

## 8. 错误替换

### 错误：

```sh
      bad substitution 

```

### 原因：

在不支持 Bash 特定语法的 shell 中使用（如`sh`）。

### 修复：

明确使用 Bash 运行脚本：

```sh
      bash script.sh 

```

**解释：** 并非所有 shell 都支持 Bash 扩展。在使用高级语法时始终使用 Bash。

## 9. 文件重定向权限被拒绝

### 错误：

```sh
      bash: output.txt: Permission denied 

```

### 原因：

用户没有权限写入文件或目录。

### 解决方法：

将输出重定向到可写位置：

```sh
      ./script.sh > ~/output.txt 

```

或者以提升的权限运行：

```sh
      sudo ./script.sh 

```

**解释：** Linux 强制执行严格的文件权限。向受保护的目录写入需要适当的权限。

## 10. 脚本未找到

### 错误：

```sh
      bash: script.sh: command not found 

```

### 原因：

脚本在没有指定相对或绝对路径的情况下执行。

### 解决方法：

```sh
      ./script.sh 

```

或者提供完整路径：

```sh
      /path/to/script.sh 

```

**解释：** Bash 只会执行它能够定位到的脚本。在需要时使用相对或绝对路径。

## 实际示例：遍历文件

初学者经常编写将目录本身而不是目录内的文件作为处理对象的循环。这是一个逻辑错误，虽然不会产生语法错误，但会产生意外的结果。

**错误：**

```sh
      for file in /home/user/docs
do
  echo Processing $file
done 

```

### 输出：

```sh
      Processing /home/user/docs 

```

**正确：**

```sh
      for file in /home/user/docs/*; do
  echo "Processing $file"
done 

```

**解释：** 添加`/*`确保循环遍历目录内的每个文件，而不是目录本身。

## 调试技巧

+   `set -x` – 在执行前显示每个命令。

+   `$?` – 检查上一个命令的退出状态。

+   `bash -n script.sh` – 在不运行脚本的情况下验证语法。

+   `bash -v script.sh` – 在执行时显示命令。

+   `2> error.log` – 将错误重定向到文件。

+   `trap 'echo "Error on line $LINENO"' ERR` – 捕获带有行号的运行时错误。

## **要点总结**

最常见的 Bash 错误大多源于：

+   缺少权限

+   语法错误

+   未设置变量

+   使用错误的 shell 运行脚本

理解这些错误并使用调试技术将帮助你编写可靠且可维护的 Bash 脚本。
