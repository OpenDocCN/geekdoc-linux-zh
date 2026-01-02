# Bash 循环（Bash Loops）

与任何其他语言一样，循环非常方便。使用 Bash，你可以使用`for`循环、`while`循环和`until`循环。

## 循环

这里是`for`循环的结构：

```sh
      for var in ${list}
do
    your_commands
done 

```

示例：

```sh
      #!/bin/bash 
users="devdojo bobby tony"

for user in ${users}
do
    echo "${user}"
done 

```

对示例的简要概述：

+   首先，我们指定一个用户列表并将值存储在一个名为`$users`的变量中。

+   然后，我们使用`for`关键字开始我们的`for`循环。

+   然后，我们定义一个新变量，它将代表我们从列表中给出的每个项目。在我们的例子中，我们定义了一个名为`user`的变量，它将代表`$users`变量中的每个用户。

+   然后，我们指定`in`关键字，后跟我们将要循环的列表。

+   在下一行，我们使用`do`关键字，它表示我们将对循环的每次迭代执行什么操作。

+   然后我们指定要运行的命令。

+   最后，我们使用`done`关键字关闭循环。

你也可以使用`for`来处理一系列数字。例如，这里是从 1 到 10 循环的一种方法：

```sh
      #!/bin/bash for num in {1..10}
do
    echo ${num}
done 

```

## 循环（While loops）

`while`循环的结构与`for`循环非常相似：

```sh
      while [ your_condition ]
do
    your_commands
done 

```

这里是一个`while`循环的示例：

```sh
      #!/bin/bash 
counter=1
while [[ $counter -le 10 ]]
do
    echo $counter
    ((counter++))
done 

```

首先，我们指定了一个计数器变量并将其设置为`1`，然后在循环内部，我们使用此语句增加计数器：`((counter++))`。这样，我们确保循环只运行 10 次，不会无限运行。一旦计数器变为 10，循环就会完成，因为这是我们设定的条件：`while [[ $counter -le 10 ]]`。

让我们创建一个脚本，询问用户他们的名字，并不允许空输入：

```sh
      #!/bin/bash read -p "What is your name? " name

while [[ -z ${name} ]]
do
    echo "Your name can not be blank. Please enter a valid name!"
    read -p "Enter your name again? " name
done

echo "Hi there ${name}" 

```

现在，如果你运行上面的代码并只按回车键而不提供输入，循环将再次运行，并反复询问你的名字，直到你实际上提供了一些输入。

## 直到循环（Until Loops）

`until`循环与`while`循环的区别在于，`until`循环将一直运行循环内的命令，直到条件变为真。

结构：

```sh
      until [[ your_condition ]]
do
    your_commands
done 

```

示例：

```sh
      #!/bin/bash 
count=1
until [[ $count -gt 10 ]]
do
    echo $count
    ((count++))
done 

```

## 继续（Continue）和中断（Break）

与其他语言一样，你可以在 bash 脚本中使用`continue`和`break`：

+   `continue`告诉你的 bash 脚本停止当前循环的迭代并开始下一次迭代。

`continue`语句的语法如下：

```sh
      continue [n] 

```

`[n]`参数是可选的，可以大于或等于 1。当提供`[n]`时，将恢复第 n 个嵌套循环。`continue 1`等同于`continue`。

```sh
      #!/bin/bash for i in 1 2 3 4 5
do
    if [[ $i -eq 2 ]] 
    then
        echo "skipping number 2"
        continue
    fi
    echo "i is equal to $i"
done 

```

我们也可以以与`break`命令类似的方式使用`continue`命令来控制多个循环。

+   `break`告诉你的 bash 脚本立即结束循环。

`break`语句的语法如下所示：

```sh
      break [n] 

```

`[n]`是一个可选参数，必须大于或等于 1。当提供`[n]`时，将退出第 n 个嵌套循环。`break 1`等同于`break`。

示例：

```sh
      #!/bin/bash 
num=1
while [[ $num -lt 10 ]] 
do
    if [[ $num -eq 5 ]] 
    then
        break
    fi
    ((num++))
done
echo "Loop completed" 

```

我们也可以使用`break`命令与多个循环一起使用。如果我们想退出当前工作循环（无论是内部循环还是外部循环），我们只需使用`break`，但如果我们在内部循环中，并想退出外部循环，我们使用`break 2`。

示例：

```sh
      #!/bin/bash for (( a = 1; a < 10; a++ ))
do
    echo "outer loop: $a"
    for (( b = 1; b < 100; b++ ))
    do
        if [[ $b -gt 5 ]] 
        then
            break 2
        fi
        echo "Inner loop: $b "
    done
done 

```

Bash 脚本将从 a=1 开始，然后进入内部循环，当它达到 b=5 时，将跳出外部循环。我们可以使用 `break` 而不是 `break 2` 来跳出内部循环，并查看它如何影响输出。
