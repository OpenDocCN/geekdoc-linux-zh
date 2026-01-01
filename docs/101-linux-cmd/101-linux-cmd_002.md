# `cd` 命令

`cd` 命令用于更改当前工作目录（即当前用户正在工作的目录）。"cd" 代表 "**c**hange **d**irectory"，它是 Linux 终端中最常用的命令之一。

`cd` 命令通常与 `ls` 命令（见第一章）结合使用，用于在系统中导航。您也可以按 `TAB` 键自动完成目录名，或者按 `TAB` 键两次以列出当前位置可用的目录。

### 语法：

```sh
      cd [OPTIONS] [directory] 

```

### 基本示例：

1.  **切换到特定目录：**

```sh
      cd /path/to/directory 

```

1.  **切换到您的家目录：**

```sh
      cd ~ 

```

OR simply:

```sh
      cd 

```

1.  **切换到上一个目录：**

```sh
      cd - 

```

这也将打印上一个目录的绝对路径。

1.  **切换到系统的根目录：**

```sh
      cd / 

```

1.  **向上移动一个目录层级（父目录）：**

```sh
      cd .. 

```

1.  **向上移动多个目录层级：**

```sh
      cd ../../.. 

```

此示例向上移动三个层级。

### 实际示例：

**使用相对路径：**

```sh
      cd Documents/Projects/MyApp 

```

**使用绝对路径：**

```sh
      cd /usr/local/bin 

```

**与家目录快捷键结合使用：**

```sh
      cd ~/Downloads 

```

**导航到具有空格名称的目录：**

```sh
      cd "My Documents" 

```

OR

```sh
      cd My\ Documents 

```

**在两个目录之间切换：**

```sh
      cd /var/log
cd /etc
cd -  # Returns to /var/log
cd -  # Returns to /etc 

```

### 其他标志及其功能

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-L` | - | 跟随符号链接（默认行为）。`cd` 命令将跟随符号链接并将工作目录更新为目标位置。 |
| `-P` | - | 使用不跟随符号链接的物理目录结构。显示实际路径而不是符号链接路径。 |

**`-L` 与 `-P` 与符号链接的示例：**

```sh
      # Assume /var/www is a symlink to /home/user/webcd -L /var/www      # Working directory shows as /var/www
pwd                 # Outputs: /var/www

cd -P /var/www      # Working directory resolves to actual path
pwd                 # Outputs: /home/user/web 

```

### 常见错误和故障排除

**权限被拒绝：**

```sh
      cd /root
# bash: cd: /root: Permission denied 

```

解决方案：您需要适当的权限来访问该目录。如果需要，请尝试使用 `sudo`。

**没有找到文件或目录：**

```sh
      cd /invalid/path
# bash: cd: /invalid/path: No such file or directory 

```

解决方案：使用 `ls` 验证路径是否存在或检查是否有拼写错误。请记住，路径区分大小写。

**不是目录：**

```sh
      cd /etc/passwd
# bash: cd: /etc/passwd: Not a directory 

```

解决方案：确保您正在导航到目录，而不是文件。

### 重要提示：

+   **大小写敏感：** Linux 文件系统区分大小写。`cd Documents` 与 `cd documents` 不同。

+   **特殊字符：** 包含空格或特殊字符的目录名需要引用或转义。

+   **`cd` 命令是 shell 内置命令：** 它不是一个外部程序，这就是为什么它可以更改 shell 的当前目录。

+   **成功时无输出：** 默认情况下，`cd` 命令在成功时不会输出任何内容（除了 `cd -` 会打印新路径）。
