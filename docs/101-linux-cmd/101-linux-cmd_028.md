# `history`命令

`history`命令显示您当前 shell 会话和过去会话中之前执行过的命令列表。这允许您回顾、搜索和重新执行命令，而无需重新输入。

### 命令语法

```sh
      history [options] [n] 

```

### 历史如何工作

您的命令历史记录存储在您的主目录中的一个文件中：

+   **Bash**：`~/.bash_history`

+   **Zsh**：`~/.zsh_history`

存储的命令数由 shell 变量控制：

+   `HISTSIZE`: 在会话期间保留在内存中的最大命令数

+   `HISTFILESIZE`: 在历史文件中保留的最大命令数

您可以使用以下命令检查这些值：

```sh
      echo $HISTSIZE
echo $HISTFILESIZE 

```

### 常见选项

+   `history n` - 显示最后`n`个命令

+   `history -c` - 清除历史记录列表（仅当前会话）

+   `history -d offset` - 删除位置`offset`的历史条目

+   `history -a` - 将新历史行追加到历史文件

+   `history -w` - 将当前历史记录写入历史文件

### 示例

**1. 显示您的完整命令历史：**

```sh
      history 

```

**2. 仅显示最后 10 个命令：**

```sh
      history 10 

```

**3. 在您的历史记录中搜索特定命令：**

```sh
      history | grep artisan
history | grep git
history | grep docker 

```

**4. 通过历史编号执行命令：**

```sh
      !123 

```

这将在您的历史记录中重新运行位置 123 的命令。

**5. 执行以特定字符串开头的最新命令：**

```sh
      !git 

```

这将运行以"git"开头的最新命令。

**6. 执行上一个命令：**

```sh
      !! 

```

**7. 使用 sudo 执行上一个命令：**

```sh
      sudo !! 

```

**8. 重复使用上一个命令的参数：**

```sh
      # If you ran: cat /var/log/syslog# You can use the last argument with:
vim !$
# This runs: vim /var/log/syslog 

```

**9. 清除您的命令历史记录：**

```sh
      history -c 

```

**10. 从历史中删除特定条目：**

```sh
      history -d 456 

```

### 反向搜索（Ctrl+R）

最强大的功能之一是**反向增量搜索**：

1.  按`Ctrl+R`

1.  开始输入命令的一部分

1.  最新的匹配命令出现

1.  再次按`Ctrl+R`以循环查看较旧匹配项

1.  按`Enter`执行，或按`Esc`编辑命令

### 历史控制变量

您可以使用这些变量在`~/.bashrc`或`~/.zshrc`中自定义历史行为：

```sh
      # Increase history sizeexport HISTSIZE=10000
export HISTFILESIZE=20000

# Ignore duplicate commands
export HISTCONTROL=ignoredups

# Ignore commands starting with a space
export HISTCONTROL=ignorespace

# Combine both options
export HISTCONTROL=ignoreboth

# Ignore specific commands from being saved
export HISTIGNORE="ls:cd:pwd:exit:history"

# Add timestamps to history
export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S " 

```

### 安全考虑

**防止敏感命令被保存：**

1.  **前缀为空格**（如果`HISTCONTROL=ignorespace`已设置）：

```sh
      mysql -u root -p 

```

1.  **暂时禁用历史记录：**

```sh
      set +o history
# Run sensitive commands here
set -o history 

```

1.  **删除特定条目：**

```sh
      history -d <line_number> 

```

1.  **在登出前清除历史记录：**

```sh
      history -c && history -w 

```

### 实际应用案例

+   **调试**：检查导致错误的命令序列

+   **文档**：复制脚本或文档中的命令序列

+   **效率**：快速重新执行复杂命令而无需重新输入

+   **学习**：在共享系统时回顾他人使用的命令（在适当的情况下）

+   **审计跟踪**：跟踪执行了哪些命令以及何时执行（启用时间戳）
