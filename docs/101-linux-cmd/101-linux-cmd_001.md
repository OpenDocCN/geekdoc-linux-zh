# `ls` 命令

`ls` 命令让您可以看到特定目录内的文件和目录（默认为当前工作目录）。它通常按升序字母顺序列出文件和目录。

### 示例：

1.  要显示当前工作目录内的文件：

```sh
      ls 

```

1.  要显示特定目录内的文件和目录：

```sh
      ls {Directory_Path} 

```

1.  以人类可读的大小列出所有文件（包括隐藏文件）的详细信息：

```sh
      ls -lah 

```

1.  按修改时间排序文件，最新的先：

```sh
      ls -lt 

```

1.  按大小列出文件，最大的先：

```sh
      ls -lhS 

```

1.  仅显示当前路径中的目录：

```sh
      ls -d */ 

```

1.  列出所有文本文件及其详细信息：

```sh
      ls -lh *.txt 

```

1.  递归列出子目录中的所有文件：

```sh
      ls -R 

```

### 语法：

```sh
      ls [-OPTION] [DIRECTORY_PATH] 

```

### 交互式训练

在这个交互式教程中，您将学习使用 `ls` 命令的不同方法：

[Tony 的 ls 命令](https://devdojo.com/tnylea/ls-command)

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-l` | - | 以长格式显示结果 |
| `-S` | - | 按文件大小排序结果 |
| `-t` | - | 按修改时间排序结果 |
| `-r` | `--reverse` | 以相反的顺序显示文件和目录（降序字母顺序） |
| `-a` | `--all` | 显示所有文件，包括隐藏文件（以点 `.` 开头的文件名） |
| `-la` | - | 显示包括隐藏文件在内的长格式文件和目录 |
| `-lh` | - | 以可读大小列出长格式文件和目录 |
| `-A` | `--almost-all` | 显示所有文件，与 `-a` 相同，但不显示 `.`（当前工作目录）和 `..`（父目录） |
| `-d` | `--directory` | 不列出目录内的文件和目录，而是显示有关目录本身的信息，可以与 `-l` 一起使用以显示长格式信息 |
| `-F` | `--classify` | 在每个列出的名称末尾附加一个指示字符，例如：在每个目录名称后附加 `/` 字符 |
| `-h` | `--human-readable` | 与 `-l` 相同，但以人类可读的单位显示文件大小，而不是字节 |
| `-i` | `--inode` | 显示每个文件的 inode 号 |
| `-R` | `--recursive` | 递归列出子目录 |
| `-1` | - | 每行列出一个文件 |
| `-c` | - | 按更改时间（文件元数据最后更改的时间）排序 |
| `-u` | - | 按访问时间（文件最后访问的时间）排序 |
| `-X` | - | 按文件扩展名字母顺序排序 |
| - | `--color=auto` | 将输出着色以区分文件类型 |
| `-g` | - | 与 `-l` 相同，但不显示所有者 |
| `-o` | - | 与 `-l` 相同，但不显示组 |
| `-C` | - | 按列列出条目 |
| `-s` | `--size` | 打印每个文件的分配大小 |
| - | `--group-directories-first` | 在文件之前列出目录 |

### 文件类型指示符：

当使用 `-F` 或 `--classify` 标志时，`ls` 会将特殊字符附加到文件名上以指示其类型：

| **指示符** | **文件类型** | **示例** |
| :-- | :-- | :-- |
| `/` | 目录 | `docs/` |
| `*` | 可执行文件 | `script.sh*` |
| `@` | 符号链接 | `link@` |
| `&#124;` | FIFO（命名管道） | `pipe&#124;` |
| `=` | 套接字 | `socket=` |
| 无指示符 | 普通文件 | `file.txt` |

**示例：**

```sh
      ls -F
documents/  script.sh*  link@  data.txt  pipe|  socket= 

```

### **基于 Red Hat 的系统上的 SELinux 支持：**

在使用基于 Red Hat 的发行版（RHEL、CentOS、Fedora、Rocky Linux、AlmaLinux）且使用 SELinux 的系统上，`ls` 命令提供了额外的选项来显示 SELinux 安全上下文信息：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-Z` | `--context` | 显示文件和目录的 SELinux 安全上下文 |
| `-lZ` | - | 显示带有 SELinux 安全上下文的长格式 |

**示例输出：**

```sh
      ls -Z
unconfined_u:object_r:user_home_t:s0 file.txt
unconfined_u:object_r:user_home_t:s0 directory 

```

```sh
      ls -lZ
-rw-rw-r--. 1 user user unconfined_u:object_r:user_home_t:s0 1234 Jan 15 10:30 file.txt
drwxrwxr-x. 2 user user unconfined_u:object_r:user_home_t:s0 4096 Jan 15 10:25 directory 

```

SELinux 上下文格式为：`用户:角色:类型:级别`

**注意：** `-Z` 选项仅在启用了 SELinux 的系统上有效。在非 SELinux 系统上，此选项可能不可用或不会显示任何附加信息。

### 理解长格式输出：

当使用 `ls -l` 时，每一行都会显示关于文件或目录的详细信息：

```sh
      -rw-r--r-- 1 user group 1234 Jan 15 10:30 file.txt 

```

**列分解：**

1.  **文件类型和权限** (`-rw-r--r--`):

    +   第一个字符：文件类型（`-` = 普通文件，`d` = 目录，`l` = 符号链接，`b` = 块设备，`c` = 字符设备，`p` = 管道，`s` = 套接字）

    +   接下来的 9 个字符：三个组（所有者、组、其他人）的权限

        +   `r` = 读取，`w` = 写入，`x` = 执行，`-` = 没有权限

1.  **链接计数** (`1`): 文件到硬链接的数量

1.  **所有者** (`user`): 文件所有者的用户名

1.  **组** (`group`): 文件所属的组名

1.  **大小** (`1234`): 文件大小（以字节为单位）（使用 `-h` 标志以人类可读格式显示，如 `1.2K`, `3.4M`）

1.  **修改时间** (`Jan 15 10:30`): 文件最后修改的日期和时间

1.  **文件名** (`file.txt`): 文件或目录的名称

**带人类可读大小的示例：**

```sh
      ls -lh
drwxr-xr-x 2 user group 4.0K Jan 15 10:25 documents
-rwxr-xr-x 1 user group 2.3M Jan 14 09:15 script.sh
-rw-r--r-- 1 user group  15K Jan 16 14:30 data.csv 

```

### 使用通配符和模式：

`ls` 命令支持通配符进行模式匹配：

| **通配符** | **描述** | **示例** | **匹配** |
| :-- | :-- | :-- | :-- |
| `*` | 匹配任意数量的字符 | `ls *.txt` | 所有以 `.txt` 结尾的文件 |
| `?` | 匹配恰好一个字符 | `ls file?.txt` | `file1.txt`, `file2.txt`，但不包括 `file10.txt` |
| `[]` | 匹配括号内的任意字符 | `ls file[123].txt` | `file1.txt`, `file2.txt`, `file3.txt` |
| `[!]` | 匹配括号外的任意字符 | `ls file[!3].txt` | `file1.txt`, `file2.txt`，但不包括 `file3.txt` |
| `{}` | 匹配任何由逗号分隔的模式 | `ls *.{jpg,png,gif}` | 所有具有这些扩展名的图像文件 |

**示例：**

```sh
      # List all PDF and DOCX files
ls *.{pdf,docx}

# List files starting with 'test' followed by a single digit
ls test?.log

# List all files starting with uppercase letters
ls [A-Z]*

# List all hidden configuration files
ls .config* 

```

### **基于 Red Hat 的系统上的 SELinux 支持：**

在使用基于 Red Hat 的发行版（RHEL、CentOS、Fedora、Rocky Linux、AlmaLinux）且使用 SELinux 的系统上，`ls` 命令提供了额外的选项来显示 SELinux 安全上下文信息：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-Z` | `--context` | 显示文件和目录的 SELinux 安全上下文 |
| `-lZ` | - | 显示带有 SELinux 安全上下文的长格式 |

**示例输出：**

```sh
      ls -Z
unconfined_u:object_r:user_home_t:s0 file.txt
unconfined_u:object_r:user_home_t:s0 directory 

```

```sh
      ls -lZ
-rw-rw-r--. 1 user user unconfined_u:object_r:user_home_t:s0 1234 Jan 15 10:30 file.txt
drwxrwxr-x. 2 user user unconfined_u:object_r:user_home_t:s0 4096 Jan 15 10:25 directory 

```

SELinux 上下文格式为：`用户:角色:类型:级别`

**注意：** `-Z` 选项仅在启用了 SELinux 的系统上有效。在非 SELinux 系统上，此选项可能不可用或不会显示任何附加信息。

### 设置持久选项：

使用 `alias` 命令在 Linux 中自定义命令行为很容易。要使这些更改永久生效，请按照以下步骤操作：

1.  **创建别名：** 使用所需选项定义您的别名。例如，要增强 `ls` 命令：

    ```sh
              alias ls="ls --color=auto -lh" 

    ```

1.  **持久性：** 此别名仅对当前会话有效。要使其永久生效，请将别名添加到您的 shell 配置文件中：

    +   **Bash：** 将别名追加到 `~/.bashrc`：

        ```sh
                      echo 'alias ls="ls --color=auto -lh"' >> ~/.bashrc
        source ~/.bashrc 

        ```

1.  **验证：** 打开一个新的终端会话，`ls` 命令将按配置显示文件。

### 性能提示：

**1. 避免在大目录上进行递归列出：**

+   在具有许多子目录和文件的目录上，`-R` 标志可能会很慢

+   考虑使用 `find` 命令以获得更多控制：`find /path -type f`

**2. 在脚本中禁用颜色输出：**

+   在管道输出或脚本中使用 `--color=never` 以提高性能

+   示例：`ls --color=never | grep pattern`

**3. 限制大目录的输出：**

+   与 `head` 结合使用，仅查看前几个条目：`ls -1 | head -n 20`

+   或者使用 `ls -1 | wc -l` 仅计算文件数量而不显示它们

**4. 在可能的情况下，使用特定路径而不是通配符：

+   当你知道确切的文件名时，`ls /var/log/syslog` 比使用 `ls /var/log/sys*` 更快

### 常见用例：

**1. 查找目录中最大的文件：**

```sh
      ls -lhS | head -n 10 

```

**2. 仅列出目录：**

```sh
      ls -d */ 

```

或者，使用完整细节：

```sh
      ls -ld */ 

```

**3. 计算目录中的文件数量：**

```sh
      ls -1 | wc -l 

```

**4. 列出过去 24 小时内修改的文件：**

```sh
      ls -lt | head 

```

**5. 按扩展名显示文件：**

```sh
      ls -lX 

```

**6. 列出带有其 inode 编号的文件（对调试很有用）：**

```sh
      ls -li 

```

**7. 每行显示目录内容（对脚本很有用）：**

```sh
      ls -1 

```

**8. 结合多个排序选项（大小 + 逆序）：**

```sh
      ls -lhSr 

```
