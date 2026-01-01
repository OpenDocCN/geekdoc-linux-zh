# `whoami` 命令

* * *

`whoami` 命令显示当前有效用户的用户名。换句话说，当执行时，它只是打印当前登录用户的用户名。

要显示您的有效用户 ID，只需在您的终端中输入 `whoami`：

```sh
      manish@godsmack:~$ whoami 
# Output:
manish 

```

语法：

```sh
      whoami [-OPTION] 

```

只有两个选项可以传递给它：

`--help`: 用于显示帮助信息并退出

示例：

```sh
      whoami --help 

```

输出结果：

```sh
      Usage: whoami [OPTION]...
Print the user name associated with the current effective user ID.
Same as id -un.

      --help     display this help and exit
      --version  output version information and exit 

```

`--version`: 输出版本信息并退出

示例：

```sh
      whoami --version 

```

输出结果：

```sh
      whoami (GNU coreutils) 8.32
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Richard Mlynarik. 

```
