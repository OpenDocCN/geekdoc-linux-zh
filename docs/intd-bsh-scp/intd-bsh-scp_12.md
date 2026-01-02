# Bash 函数

函数是重用代码的绝佳方式。Bash 中函数的结构与大多数语言相当相似：

```sh
      function function_name() {
    your_commands
} 

```

您也可以省略开头的`function`关键字，这同样有效：

```sh
      function_name() {
    your_commands
} 

```

我更喜欢将其放在那里以提高可读性。但这完全是个人的偏好问题。

“Hello World!”函数的示例：

```sh
      #!/bin/bash function hello() {
    echo "Hello World Function!"
}

hello 

```

> **注意：** 需要注意的一点是，在调用函数时不应添加括号。

向函数传递参数的方式与向脚本传递参数的方式相同：

```sh
      #!/bin/bash function hello() {
    echo "Hello $1!"
}

hello DevDojo 

```

函数应该包含注释，说明描述、全局变量、参数、输出和返回值（如有适用）。

```sh
      ######################################## Description: Hello function# Globals:#   None# Arguments:#   Single input argument# Outputs:#   Value of input argument# Returns:#   0 if successful, non-zero on error.#######################################function hello() {
    echo "Hello $1!"
} 

```

在接下来的几章中，我们将大量使用函数！
