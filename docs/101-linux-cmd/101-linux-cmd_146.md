# `ufw` 命令

UFW（简化防火墙）是 Ubuntu 和其他基于 Debian 的系统上管理 iptables 防火墙规则的友好命令行前端。它提供了一种简单的方式来配置防火墙规则，无需处理复杂的 iptables 语法。

## 语法

```sh
      ufw [options] command [parameters] 

```

## 安装

```sh
      # Ubuntu/Debian
sudo apt update && sudo apt install ufw

# Check if UFW is installed
which ufw 

```

## 基本命令

### 启用/禁用 UFW

```sh
      # Enable UFW
sudo ufw enable

# Disable UFW
sudo ufw disable

# Check UFW status
sudo ufw status
sudo ufw status verbose
sudo ufw status numbered 

```

## 基本规则

### 允许/拒绝流量

1.  **允许特定端口**

```sh
      # Allow SSH (port 22)
sudo ufw allow 22
sudo ufw allow ssh

# Allow HTTP (port 80)
sudo ufw allow 80
sudo ufw allow http

# Allow HTTPS (port 443)
sudo ufw allow 443
sudo ufw allow https

# Allow custom port
sudo ufw allow 8080 

```

1.  **拒绝特定端口**

```sh
      # Deny port 80
sudo ufw deny 80

# Deny SSH from specific IP
sudo ufw deny from 192.168.1.100 to any port 22 

```

1.  **通过服务名称允许/拒绝**

```sh
      # Allow common services
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw allow ftp
sudo ufw allow smtp 

```

## 高级规则

### 端口范围

```sh
      # Allow port range
sudo ufw allow 1000:2000/tcp
sudo ufw allow 1000:2000/udp

# Allow specific protocol
sudo ufw allow 53/udp  # DNS
sudo ufw allow 53/tcp  # DNS over TCP 

```

### IP 地址规则

1.  **允许/拒绝特定 IP 地址**

```sh
      # Allow from specific IP
sudo ufw allow from 192.168.1.100

# Deny from specific IP
sudo ufw deny from 192.168.1.50

# Allow subnet
sudo ufw allow from 192.168.1.0/24 

```

1.  **允许 IP 到特定端口**

```sh
      # Allow specific IP to SSH
sudo ufw allow from 192.168.1.100 to any port 22

# Allow subnet to web server
sudo ufw allow from 10.0.0.0/8 to any port 80 

```

### 接口特定规则

```sh
      # Allow on specific interface
sudo ufw allow in on eth0 to any port 80

# Allow out on specific interface
sudo ufw allow out on eth1 to any port 443 

```

## 规则管理

### 列出规则

```sh
      # Show status and rules
sudo ufw status

# Show numbered rules
sudo ufw status numbered

# Show verbose status
sudo ufw status verbose 

```

### 删除规则

```sh
      # Delete by rule number
sudo ufw delete 3

# Delete by specifying the rule
sudo ufw delete allow 80
sudo ufw delete allow from 192.168.1.100 

```

### 插入规则

```sh
      # Insert rule at specific position
sudo ufw insert 1 allow from 192.168.1.0/24 

```

## 默认策略

```sh
      # Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw default deny forward

# Check current defaults
sudo ufw status verbose 

```

## 应用程序配置文件

### 列出可用配置文件

```sh
      # List application profiles
sudo ufw app list

# Show profile info
sudo ufw app info OpenSSH
sudo ufw app info "Apache Full" 

```

### 使用应用程序配置文件

```sh
      # Allow application
sudo ufw allow OpenSSH
sudo ufw allow "Apache Full"
sudo ufw allow "Nginx Full"

# Common application profiles
sudo ufw allow "OpenSSH"
sudo ufw allow "Apache"
sudo ufw allow "Apache Secure"
sudo ufw allow "Nginx HTTP"
sudo ufw allow "Nginx HTTPS"
sudo ufw allow "Nginx Full" 

```

## 日志记录

```sh
      # Enable logging
sudo ufw logging on

# Set log level
sudo ufw logging low
sudo ufw logging medium
sudo ufw logging high

# Disable logging
sudo ufw logging off

# View logs
sudo tail -f /var/log/ufw.log 

```

## 重置和重新加载

```sh
      # Reset all rules to default
sudo ufw --force reset

# Reload UFW
sudo ufw reload 

```

## 常见用例

### 1. 基本网络服务器设置

```sh
      # Allow SSH, HTTP, and HTTPS
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw enable 

```

### 2. 数据库服务器（MySQL）

```sh
      # Allow MySQL only from application servers
sudo ufw allow from 192.168.1.10 to any port 3306
sudo ufw allow from 192.168.1.11 to any port 3306 

```

### 3. 开发服务器

```sh
      # Allow common development ports
sudo ufw allow 3000  # Node.js
sudo ufw allow 8000  # Django
sudo ufw allow 5000  # Flask
sudo ufw allow 4200  # Angular 

```

### 4. 邮件服务器

```sh
      # Allow mail server ports
sudo ufw allow smtp      # Port 25
sudo ufw allow 587/tcp   # SMTP submission
sudo ufw allow 993/tcp   # IMAPS
sudo ufw allow 995/tcp   # POP3S 

```

### 5. DNS 服务器

```sh
      # Allow DNS traffic
sudo ufw allow 53/tcp
sudo ufw allow 53/udp 

```

## 安全最佳实践

### 1. 最小权限原则

```sh
      # Start with deny all
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Only allow what's needed
sudo ufw allow ssh
sudo ufw allow from 192.168.1.0/24 to any port 80 

```

### 2. 限制 SSH 访问

```sh
      # Limit SSH attempts (6 attempts in 30 seconds)
sudo ufw limit ssh

# Allow SSH only from specific networks
sudo ufw allow from 192.168.1.0/24 to any port 22
sudo ufw deny ssh 

```

### 3. 监控和日志

```sh
      # Enable logging
sudo ufw logging medium

# Monitor logs
sudo tail -f /var/log/ufw.log | grep DPT 

```

## 故障排除

### 1. 检查当前规则

```sh
      sudo ufw status numbered
sudo iptables -L -n 

```

### 2. 测试连接

```sh
      # Test if port is accessible
telnet your-server-ip 80
nc -zv your-server-ip 22 

```

### 3. 调试 UFW

```sh
      # Dry run (show what would happen)
sudo ufw --dry-run allow 80

# Check UFW version
ufw --version 

```

## 高级配置

### 1. 自定义规则文件

```sh
      # Edit UFW rules directly
sudo vim /etc/ufw/user.rules
sudo vim /etc/ufw/user6.rules 

```

### 2. 速率限制

```sh
      # Limit connections per IP
sudo ufw limit ssh
sudo ufw limit 80/tcp 

```

### 3. 端口转发

```sh
      # Enable IP forwardingecho 'net.ipv4.ip_forward=1' | sudo tee -a /etc/ufw/sysctl.conf

# Add NAT rules to /etc/ufw/before.rules 

```

## 服务集成

### 1. Docker 集成

```sh
      # Allow Docker containers
sudo ufw allow from 172.17.0.0/16

# Block Docker bypass (in /etc/ufw/after.rules) 

```

### 2. Fail2ban 集成

```sh
      # UFW works with fail2ban
sudo apt install fail2ban
# Configure fail2ban to use UFW actions 

```

## 重要注意事项

+   UFW 是 iptables 的前端，而不是替代品

+   规则按顺序处理（先匹配者胜出）

+   当没有特定规则匹配时，应用默认策略

+   默认情况下，UFW 不会干扰现有的 iptables 规则

+   在生产环境中启用规则之前，始终测试规则

+   在远程启用 UFW 之前，保留 SSH 访问规则

## 快速参考

```sh
      # Essential commands
sudo ufw enable                    # Enable firewall
sudo ufw status                    # Check status
sudo ufw allow 22                  # Allow SSH
sudo ufw allow from 192.168.1.0/24 # Allow subnet
sudo ufw delete 3                  # Delete rule #3
sudo ufw reset                     # Reset all rules
sudo ufw reload                    # Reload configuration 

```

UFW 在简单性和功能之间提供了极佳的平衡，使其成为需要有效防火墙管理而不必处理 iptables 复杂性的系统管理员的理想选择。

更多详细信息，请查看手册：`man ufw`
