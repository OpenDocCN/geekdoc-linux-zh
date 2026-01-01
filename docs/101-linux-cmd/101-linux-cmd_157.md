# `!!` 命令（历史扩展）

`!!` 命令是 bash 历史扩展功能，用于重复最后一个命令。它是历史扩展能力更广泛集合的一部分，允许您快速重新执行、修改或引用之前的命令。这对于纠正错误、添加 sudo 或重复复杂命令非常有用。

## 语法

```sh
      !!
!<event>
!<string>
!?<string>? 

```

## 关键特性

+   **重复上一个命令**：快速重新运行上一个命令

+   **命令修改**：修改并重新执行之前的命令

+   **参数提取**：从之前的命令中提取特定参数

+   **模式匹配**：通过模式查找命令

+   **节省时间**：避免重新输入复杂的命令

## 基本用法

### 简单历史扩展

```sh
      # Run a command
ls -la /home/user

# Repeat the last command
!!

# Add sudo to the last command
sudo !!

# This is equivalent to:
sudo ls -la /home/user 

```

### 常见场景

```sh
      # Forgot sudo
apt update
# Permission denied

# Fix with sudo
sudo !!
# Executes: sudo apt update

# Wrong directory
cd /var/logs
# No such file or directory

# Fix the path
cd /var/log
# Then repeat with correct path
!! 

```

## 历史扩展模式

### 1. 事件指示符

```sh
      # !! - Last command
!!

# !n - Command number n
!123

# !-n - nth command back
!-2    # Two commands ago
!-5    # Five commands ago

# !string - Most recent command starting with 'string'
!ls    # Last command starting with 'ls'
!git   # Last command starting with 'git'

# !?string? - Most recent command containing 'string'
!?config?   # Last command containing 'config'
!?file?     # Last command containing 'file' 

```

### 2. 单词指示符

```sh
      # Previous command: git commit -m "Fix bug" --amend# !^ - First argumentecho !^        # echo commit

# !$ - Last argument
echo !$        # echo --amend

# !* - All arguments
echo !*        # echo commit -m "Fix bug" --amend

# !:n - nth argument (0-based)
echo !:0       # echo git
echo !:1       # echo commit
echo !:2       # echo -m

# !:n-m - Range of arguments
echo !:1-3     # echo commit -m "Fix bug" 

```

### 3. 修饰符

```sh
      # Previous command: ls /home/user/documents/file.txt# :h - Remove trailing pathname component (head)echo !!:h      # echo /home/user/documents

# :t - Remove leading pathname components (tail)
echo !!:t      # echo file.txt

# :r - Remove trailing suffix
echo !!:r      # echo /home/user/documents/file

# :e - Remove all but trailing suffix
echo !!:e      # echo txt

# :s/old/new/ - Substitute first occurrence
!!:s/user/admin/   # ls /home/admin/documents/file.txt

# :gs/old/new/ - Global substitution
!!:gs/o/0/         # ls /h0me/user/d0cuments/file.txt 

```

## 实际示例

### 1. 错误纠正

```sh
      # Typo in command
ct /etc/hosts
# Command not found

# Correct and re-run
cat /etc/hosts

# Then if you need to edit it
sudo vi !!    # Becomes: sudo vi /etc/hosts 

```

### 2. 添加缺失的选项

```sh
      # Run command without verbose
tar -czf backup.tar.gz /home/user

# Add verbose flag
!!:s/-czf/-czvf/
# Becomes: tar -czvf backup.tar.gz /home/user

# Or simpler approach
tar -czvf backup.tar.gz /home/user 

```

### 3. 文件操作

```sh
      # Create file
touch /tmp/test_file.txt

# Edit the same file
vi !!:$     # vi /tmp/test_file.txt

# Copy to different location
cp !!:$ /home/user/
# Becomes: cp /tmp/test_file.txt /home/user/

# Previous command: find /var/log -name "*.log" -size +10M
# Copy found files
cp !!:3 !!:4 !!:5 /backup/
# Using specific arguments from find command 

```

### 4. 目录导航

```sh
      # Change to a directorycd /usr/local/bin

# List contents
ls !!:$     # ls /usr/local/bin

# Go back and then to related directory
cd /usr/local/src
# ... later ...
cd !!:h/bin    # cd /usr/local/bin 

```

## 高级历史扩展

### 1. 复杂替换

```sh
      # Previous: rsync -av /home/user/docs/ backup@server:/backup/user/docs/# Change source directory
!!:s/docs/pictures/
# Becomes: rsync -av /home/user/pictures/ backup@server:/backup/user/docs/

# Change both source and destination
!!:s/docs/pictures/:s/user\/docs/user\/pictures/
# Global changes
!!:gs/docs/pictures/ 

```

### 2. 与其他功能结合

```sh
      # Previous: find /var/log -name "*.log" -type f -exec ls -l {} \;# Modify to use different action
!!:s/-exec ls -l/-exec wc -l/
# Count lines instead of listing

# Extract just the find portion
!:0-4    # find /var/log -name "*.log" -type f

# Use arguments with different command
grep "error" !!:3  # grep "error" "*.log" 

```

### 3. 使用多个命令

```sh
      # Pipeline command
ps aux | grep apache | grep -v grep

# Modify the grep pattern
!!:s/apache/nginx/
# Becomes: ps aux | grep nginx | grep -v grep

# Extract just part of pipeline
!:0-2     # ps aux | grep apache

# Add to existing pipeline
!! | wc -l    # Count the results 

```

## 交互式功能

### 1. 历史验证

```sh
      # Enable history verification (before execution)set +H    # Disable history expansion
set -H    # Enable history expansion

# Show command before execution
shopt -s histverify
# Now !! will show the command and wait for Enter 

```

### 2. 历史搜索

```sh
      # Ctrl+R - Reverse search# Type to search through history# Ctrl+R again to find previous matches# Search for specific command
!?git commit?    # Find last command containing "git commit"
!?ssh?           # Find last command containing "ssh" 

```

## 配置和设置

### 1. 历史设置

```sh
      # History sizeexport HISTSIZE=1000        # Commands in memory
export HISTFILESIZE=2000    # Commands in file

# History control
export HISTCONTROL=ignoreboth    # Ignore duplicates and spaces
export HISTCONTROL=ignoredups    # Ignore duplicates only

# History ignore patterns
export HISTIGNORE="ls:cd:cd -:pwd:exit:date:* --help"

# Timestamp in history
export HISTTIMEFORMAT="%F %T " 

```

### 2. 历史扩展设置

```sh
      # Check if history expansion is enabledset +o | grep histexpand

# Enable history expansion
set -H
# or
set -o histexpand

# Disable history expansion
set +H
# or
set +o histexpand 

```

## 安全性和最佳实践

### 1. 执行前验证

```sh
      # Enable command verificationshopt -s histverify

# This makes !! show the command first, requiring Enter to execute

# Check what command will be executed
history | tail -1    # See last command
echo !!             # See what !! would execute (without running it) 

```

### 2. 安全实践

```sh
      # Be careful with destructive commands
rm -rf /tmp/*
# Don't blindly run !! after such commands

# Use history to verify
history | tail -5    # Check recent commands

# For critical operations, type commands fully
# Don't rely on history expansion for:
# - rm commands
# - chmod commands
# - System configuration changes 

```

### 3. 调试

```sh
      # See history expansion in actionset -x    # Enable command tracing
!!        # You'll see the expanded command
set +x    # Disable tracing

# Check history before using
history 10    # Show last 10 commands
!-2           # Run 2nd to last command 

```

## 常见模式和快捷键

### 1. 常见组合

```sh
      # Add sudo to last command
sudo !!

# Redirect last command output
!! > output.txt
!! 2>&1 | tee log.txt

# Background last command
!! &

# Time last command
time !!

# Run last command in different directory
(cd /tmp && !!) 

```

### 2. 文件和目录操作

```sh
      # Previous: vi /etc/apache2/sites-available/default# Test the configuration
apache2ctl -t

# Copy the file
cp !!:$ !!:$:r.backup    # cp /etc/apache2/sites-available/default /etc/apache2/sites-available/default.backup

# Edit related file
vi !!:h/sites-enabled/default    # vi /etc/apache2/sites-enabled/default 

```

### 3. 网络和系统命令

```sh
      # Check service status
systemctl status apache2

# Restart if needed
sudo !!:s/status/restart/    # sudo systemctl restart apache2

# Check multiple services
systemctl status nginx
!!:s/nginx/mysql/           # systemctl status mysql
!!:s/mysql/postgresql/      # systemctl status postgresql 

```

## 与脚本集成

### 1. 脚本中的历史

```sh
      #!/bin/bash# Note: History expansion doesn't work in scripts by default# Enable it explicitly if neededset -H    # Enable history expansion in script

# Use variables instead of history expansion in scripts
last_command="$1"
echo "Re-running: $last_command"
eval "$last_command" 

```

### 2. 交互式脚本

```sh
      #!/bin/bash# Interactive script using history conceptswhile true; do
    read -p "Command: " cmd

    if [ "$cmd" = "!!" ]; then
        echo "Re-running: $last_cmd"
        eval "$last_cmd"
    elif [ "$cmd" = "exit" ]; then
        break
    else
        eval "$cmd"
        last_cmd="$cmd"
    fi
done 

```

## 替代命令和相关命令

### 1. fc（修复命令）

```sh
      # Edit last command in editorfc# Edit specific command numberfc 123

# List recent commands
fc -l

# Re-run range of commands
fc -s 100 105 

```

### 2. history 命令

```sh
      # Show command historyhistory# Show last 10 commandshistory 10

# Execute specific command number
!123

# Search and execute
history | grep git
!456 

```

## 故障排除

### 1. 历史扩展不起作用

```sh
      # Check if enabledset +o | grep histexpand

# Enable it
set -H

# Check in current shell
echo $-    # Should contain 'H' 

```

### 2. 预期之外的扩展

```sh
      # Escape ! to prevent expansionecho "The price is \$5\!"

# Use single quotes
echo 'The price is $5!'

# Disable temporarily
set +H
echo "Commands with ! work normally"
set -H 

```

### 3. 复杂命令问题

```sh
      # For very complex commands, use variables
complex_cmd="find /var/log -name '*.log' -exec grep 'error' {} \;"
eval "$complex_cmd"

# Then modify variable instead of using history expansion
complex_cmd="${complex_cmd/error/warning}"
eval "$complex_cmd" 

```

## 重要注意事项

+   **仅交互式**：历史扩展主要在交互式 shell 中工作

+   **不在脚本中**：通常在脚本中禁用以提高安全性

+   **特定于 shell**：这是一个 bash/zsh 特性，并非所有 shell 都可用

+   **验证**：使用 `histverify` 选项在具有破坏性的命令中提高安全性

+   **大小写敏感**：历史扩展是大小写敏感的

+   **立即执行**：!! 立即执行；在使用具有破坏性的命令时要小心

`!!` 命令和历史扩展功能是高效的命令行工具，但它们需要理解和谨慎使用以避免错误。

更多详细信息，请查看 bash 手册：`man bash`（搜索“历史扩展”）
