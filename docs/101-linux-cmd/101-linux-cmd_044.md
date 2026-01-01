# `su` 命令

`su`（替换用户）命令允许您以其他用户账户运行命令。它通常用于切换到 root 账户以执行管理任务，或者在不注销和重新登录的情况下以不同用户身份运行命令。

## 语法

```sh
      su [OPTIONS] [-] [USER [ARGUMENT...]] 

```

## 关键特性

+   **用户切换**：切换到系统上的任何用户账户

+   **环境控制**：选择是否继承或重置环境变量

+   **Shell 选择**：指定要使用的 shell

+   **组管理**：切换主要和次要组

+   **命令执行**：以其他用户身份运行特定命令

## 基本用法

### 切换到 Root

```sh
      # Switch to root user (requires root password)
su

# Switch to root with login shell (recommended)
su -

# Alternative syntax for login shell
su -l
su --login 

```

### 切换到特定用户

```sh
      # Switch to specific user
su username

# Switch to user with login shell
su - username
su -l username

# Switch to user and run specific command
su - username -c "whoami" 

```

## 环境处理

### 登录 Shell 与非登录 Shell

```sh
      # Non-login shell (keeps current environment)
su username
# Current directory and environment variables are preserved

# Login shell (starts fresh environment)
su - username
# Changes to user's home directory and loads their profile 

```

### 环境变量控制

```sh
      # Preserve current environment
su -m username
su --preserve-environment username

# Preserve specific environment variables
su -w HOME,TERM username
su --whitelist-environment=HOME,TERM username

# Reset environment but preserve specific variables
su - username --preserve-environment=PATH 

```

## 高级用法

### 组管理

```sh
      # Switch user and primary group
su -g developers username

# Switch with supplementary groups
su -G developers,admins username

# Check current groups
su - username -c "groups" 

```

### Shell 选择

```sh
      # Use specific shell
su -s /bin/bash username
su --shell=/bin/zsh username

# Use shell if allowed by /etc/shells
su -s /usr/bin/fish username

# Check available shells
cat /etc/shells 

```

### 命令执行

```sh
      # Run single command as another user
su - username -c "ls -la /home/username"

# Run multiple commands
su - username -c "cd /tmp && ls -la && pwd"

# Run script as another user
su - username -c "/path/to/script.sh"

# Run command with arguments
su - username -c "grep 'pattern' /var/log/syslog" 

```

## 实际示例

### 系统管理

```sh
      # Switch to root for administrative tasks
su -
# Now you can run administrative commands

# Run single administrative command
su -c "systemctl restart apache2"

# Edit system configuration file
su -c "nano /etc/hosts"

# Check system logs
su -c "tail -f /var/log/syslog" 

```

### 开发工作流程

```sh
      # Switch to application user for deployment
su - appuser -c "cd /opt/myapp && ./deploy.sh"

# Run application as specific user
su - www-data -c "/usr/bin/php /var/www/html/script.php"

# Test permissions as different user
su - testuser -c "ls -la /shared/directory" 

```

### 用户管理任务

```sh
      # Create file as specific user
su - username -c "touch /home/username/newfile.txt"

# Check user's environment
su - username -c "env | sort"

# Run user's shell configuration
su - username -c "source ~/.bashrc && echo \$PATH" 

```

## 安全考虑

### 密码要求

```sh
      # su requires the target user's password
su username  # Requires username's password

# Root can switch to any user without password
sudo su - username  # Uses sudo authentication

# Check who can use su
grep su /etc/group 

```

### 审计和日志记录

```sh
      # Check su usage in logs
sudo grep su /var/log/auth.log

# Monitor current su sessions
w
who

# Check login history
last 

```

### 安全使用模式

```sh
      # Always use login shell for administrative tasks
su -  # Better than just 'su'

# Use sudo instead of su when possible
sudo command  # Better than 'su -c command'

# Limit time as root
su -c "command1 && command2 && exit" 

```

## 与 Sudo 的比较

### 何时使用 `su`

```sh
      # Multiple administrative commands
su -
# Run several commands as root
exit

# Interactive root session
su -
# Work as root for extended period 

```

### 何时使用 `sudo`

```sh
      # Single command execution
sudo systemctl restart service

# Better security and logging
sudo -u username command

# Temporary privilege escalation
sudo apt update && sudo apt upgrade 

```

## 配置和自定义

### PAM 配置

```sh
      # Check PAM configuration for su
cat /etc/pam.d/su

# Restrict su to wheel group (some distributions)
# Edit /etc/pam.d/su and uncomment:
# auth required pam_wheel.so use_uid 

```

### Shell 配置

```sh
      # Check if shell is allowed
grep username /etc/passwd
cat /etc/shells

# Set shell for user
sudo chsh -s /bin/bash username 

```

### 环境自定义

```sh
      # Customize login environment
        # Edit ~/.profile, ~/.bashrc, or ~/.bash_profile
        # Set specific environment for su sessions
        # Create ~/.surc or modify shell configuration 

```

## 故障排除

### 常见问题

```sh
      # Authentication failure
su: Authentication failure
# Check password, user existence, account status

# Permission denied
su: Permission denied
# Check PAM configuration, wheel group membership

# Shell not allowed
su: Warning: shell not allowed
# Add shell to /etc/shells or use -s option 

```

### 调试

```sh
      # Check user account status
sudo passwd -S username

# Verify user existence
id username
grep username /etc/passwd

# Check group membership
groups username

# Test with verbose output
su -v username 

```

## 脚本集成

### 在脚本中使用 su

```sh
      #!/bin/bash# Script to run commands as different userif [ "$EUID" -ne 0 ]; then
    echo "Please run as root"
    exit 1
fi

# Switch to app user and run commands
su - appuser -c "
    cd /opt/myapp
    ./backup.sh
    ./cleanup.sh
" 

```

### 自动化任务

```sh
      # Cron job running as specific user# Add to root's crontab:# 0 2 * * * su - backupuser -c '/usr/local/bin/backup.sh'# System service running as user
su - serviceuser -c '/opt/service/start.sh' & 

```

## 选项参考

| **选项** | **长格式** | **描述** |
| :-- | :-- | :-- |
| `-` | `--login` | 启动登录 shell，加载用户的环境 |
| `-c CMD` | `--command=CMD` | 执行命令并退出 |
| `-f` | `--fast` | 将 -f 传递给 shell（用于 csh/tcsh） |
| `-g GRP` | `--group=GRP` | 指定主要组 |
| `-G GRP` | `--supp-group=GRP` | 指定次要组 |
| `-l` | `--login` | 与 `-` 选项相同 |
| `-m` | `--preserve-environment` | 不重置环境变量 |
| `-p` | `--preserve-environment` | 与 `-m` 选项相同 |
| `-s SHL` | `--shell=SHL` | 使用指定的 shell |
| `-w VAR` | `--whitelist-environment=VAR` | 不重置指定的变量 |
| `--help` | - | 显示帮助信息 |
| `--version` | - | 显示版本信息 |

## 最佳实践

### 安全最佳实践

+   当可能时，使用 `sudo` 而不是 `su` 以获得更好的日志记录

+   始终使用登录 shell (`su -`) 进行管理任务

+   限制作为 root 用户的时间

+   当可能时，使用特定命令而不是交互式会话

+   定期通过系统日志审计 su 使用情况

### 运营最佳实践

+   在脚本中切换用户时使用描述性注释

+   在尝试切换之前验证用户存在

+   在脚本中优雅地处理认证失败

+   在系统文档中记录用户切换要求

## 重要提示

+   `su` 需要目标用户的密码（除非以 root 身份运行）

+   建议使用 `su -` 进行管理任务，因为它提供了一个干净的环境

+   `su` 会话记录在 `/var/log/auth.log` 或类似的系统日志中

+   在某些系统上可能启用了 `wheel` 组限制

+   完成后始终退出 su 会话以返回到原始用户

`su` 命令对于 Linux 系统中的用户切换和权限管理至关重要，但应谨慎使用，并考虑适当的安全措施。

想要了解更多详情，请查阅手册：`man su`
