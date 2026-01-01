# `iptables` 命令

`iptables` 命令是 Linux 系统的强大防火墙管理工具。它允许您通过设置、维护和检查 IP 数据包过滤规则表来配置 Linux 内核防火墙（netfilter）。

## 语法

```sh
      iptables [options] [chain] [rule-specification] [target] 

```

## 基本概念

### 表

+   **filter**: 数据包过滤的默认表（INPUT, OUTPUT, FORWARD）

+   **nat**: 网络地址转换（PREROUTING, POSTROUTING, OUTPUT）

+   **mangle**: 数据包修改（PREROUTING, POSTROUTING, INPUT, OUTPUT, FORWARD）

+   **raw**: 连接跟踪豁免（PREROUTING, OUTPUT）

### 链

+   **INPUT**: 发往本地系统的入站数据包

+   **OUTPUT**: 来自本地系统的出站数据包

+   **FORWARD**: 通过系统路由的数据包

+   **PREROUTING**: 路由决策之前的数据包

+   **POSTROUTING**: 路由决策之后的数据包

### 目标

+   **ACCEPT**: 允许数据包

+   **DROP**: 静默丢弃数据包

+   **REJECT**: 丢弃并发送错误消息

+   **LOG**: 记录数据包并继续处理

+   **DNAT**: 目标网络地址转换

+   **SNAT**: 源网络地址转换

+   **MASQUERADE**: 动态源网络地址转换

## 基本命令

### 列出规则

```sh
      # List all rules
sudo iptables -L

# List rules with line numbers
sudo iptables -L --line-numbers

# List rules in specific table
sudo iptables -t nat -L
sudo iptables -t mangle -L

# Show packet and byte counters
sudo iptables -L -v

# Show rules in iptables-save format
sudo iptables -S 

```

### 基本规则操作

```sh
      # Add rule to end of chain
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Insert rule at specific position
sudo iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT

# Delete specific rule
sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT

# Delete rule by line number
sudo iptables -D INPUT 3

# Replace rule at specific position
sudo iptables -R INPUT 1 -p tcp --dport 443 -j ACCEPT 

```

## 常见规则示例

### 1. 允许/阻止特定端口

```sh
      # Allow SSH (port 22)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP (port 80)
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Allow HTTPS (port 443)
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Block specific port
sudo iptables -A INPUT -p tcp --dport 8080 -j DROP 

```

### 2. 通过 IP 地址允许/阻止

```sh
      # Allow specific IP
sudo iptables -A INPUT -s 192.168.1.100 -j ACCEPT

# Block specific IP
sudo iptables -A INPUT -s 192.168.1.50 -j DROP

# Allow subnet
sudo iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT

# Block IP range
sudo iptables -A INPUT -m iprange --src-range 192.168.1.100-192.168.1.200 -j DROP 

```

### 3. 通过接口允许/阻止

```sh
      # Allow traffic on loopback
sudo iptables -A INPUT -i lo -j ACCEPT

# Allow on specific interface
sudo iptables -A INPUT -i eth0 -p tcp --dport 80 -j ACCEPT

# Block on specific interface
sudo iptables -A INPUT -i eth1 -j DROP 

```

## 高级规则

### 1. 状态连接

```sh
      # Allow established and related connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow new connections on specific ports
sudo iptables -A INPUT -m state --state NEW -p tcp --dport 22 -j ACCEPT 

```

### 2. 速率限制

```sh
      # Limit SSH connections (6 per minute)
sudo iptables -A INPUT -p tcp --dport 22 -m limit --limit 6/min -j ACCEPT

# Limit ping requests
sudo iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 1/sec -j ACCEPT 

```

### 3. 基于时间的规则

```sh
      # Allow access during business hours
sudo iptables -A INPUT -p tcp --dport 80 -m time --timestart 09:00 --timestop 17:00 -j ACCEPT

# Allow access on weekdays
sudo iptables -A INPUT -p tcp --dport 22 -m time --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT 

```

### 4. 多端口规则

```sh
      # Allow multiple ports
sudo iptables -A INPUT -p tcp -m multiport --dports 22,80,443 -j ACCEPT

# Block multiple ports
sudo iptables -A INPUT -p tcp -m multiport --dports 135,445,1433 -j DROP 

```

## NAT 配置

### 1. 源网络地址转换（SNAT）

```sh
      # Static SNAT
sudo iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j SNAT --to-source 203.0.113.1

# Dynamic SNAT (Masquerading)
sudo iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j MASQUERADE 

```

### 2. 目标网络地址转换（DNAT）

```sh
      # Port forwarding
sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 192.168.1.100:80

# Forward to different IP
sudo iptables -t nat -A PREROUTING -d 203.0.113.1 -j DNAT --to-destination 192.168.1.100 

```

## 政策配置

### 默认策略

```sh
      # Set default policies
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# View current policies
sudo iptables -L | grep "policy" 

```

### 链管理

```sh
      # Create custom chain
sudo iptables -N CUSTOM_CHAIN

# Delete custom chain (must be empty)
sudo iptables -X CUSTOM_CHAIN

# Flush specific chain
sudo iptables -F INPUT

# Flush all chains
sudo iptables -F 

```

## 记录

```sh
      # Log dropped packets
sudo iptables -A INPUT -j LOG --log-prefix "DROPPED: " --log-level 4

# Log before dropping
sudo iptables -A INPUT -p tcp --dport 23 -j LOG --log-prefix "TELNET_ATTEMPT: "
sudo iptables -A INPUT -p tcp --dport 23 -j DROP

# View logs
sudo tail -f /var/log/syslog | grep "DROPPED:" 

```

## 常见防火墙配置

### 1. 基本桌面防火墙

```sh
      #!/bin/bash# Clear existing rules
sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t nat -X

# Default policies
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Allow loopback
sudo iptables -A INPUT -i lo -j ACCEPT

# Allow established connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow ping
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT 

```

### 2. 网络服务器防火墙

```sh
      #!/bin/bash# Basic web server configuration
sudo iptables -F

# Default policies
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Allow loopback and established connections
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow SSH (limit attempts)
sudo iptables -A INPUT -p tcp --dport 22 -m limit --limit 6/min -j ACCEPT

# Allow HTTP/HTTPS
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow ping
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT 

```

### 3. 路由器/网关配置

```sh
      #!/bin/bash# Enable IP forwardingecho 1 > /proc/sys/net/ipv4/ip_forward

# NAT for internal network
sudo iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j MASQUERADE

# Allow forwarding for established connections
sudo iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow forwarding from internal network
sudo iptables -A FORWARD -s 192.168.1.0/24 -j ACCEPT 

```

## 持久性

### 1. 保存/恢复规则

```sh
      # Save current rules
sudo iptables-save > /etc/iptables/rules.v4

# Restore rules
sudo iptables-restore < /etc/iptables/rules.v4

# Install persistence package (Ubuntu/Debian)
sudo apt install iptables-persistent 

```

### 2. 自动加载

```sh
      # Create systemd service
sudo vim /etc/systemd/system/iptables-restore.service

[Unit]
Description=Restore iptables firewall rules
Before=network-pre.target

[Service]
Type=oneshot
ExecStart=/sbin/iptables-restore /etc/iptables/rules.v4

[Install]
WantedBy=multi-user.target

# Enable service
sudo systemctl enable iptables-restore.service 

```

## 故障排除

### 1. 测试规则

```sh
      # Test connectivity
telnet target-ip port
nc -zv target-ip port

# Check if rule matches
sudo iptables -L -v -n | grep "rule-description"

# Monitor rule usage
watch "sudo iptables -L -v -n" 

```

### 2. 调试

```sh
      # Enable all logging temporarily
sudo iptables -A INPUT -j LOG --log-prefix "INPUT: "
sudo iptables -A OUTPUT -j LOG --log-prefix "OUTPUT: "
sudo iptables -A FORWARD -j LOG --log-prefix "FORWARD: "

# Monitor logs
sudo tail -f /var/log/syslog | grep "INPUT:\|OUTPUT:\|FORWARD:" 

```

### 3. 紧急访问

```sh
      # Temporary rule to allow all (emergency)
sudo iptables -I INPUT 1 -j ACCEPT

# Flush all rules (removes all protection)
sudo iptables -F
sudo iptables -X
sudo iptables -P INPUT ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -P FORWARD ACCEPT 

```

## 安全最佳实践

### 1. 默认拒绝策略

```sh
      # Always start with deny-all policy
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
# Keep OUTPUT as ACCEPT for normal operation 

```

### 2. 顺序问题

```sh
      # More specific rules should come first
sudo iptables -I INPUT 1 -s 192.168.1.100 -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j DROP 

```

### 3. 限制关键服务的速率

```sh
      # Protect SSH from brute force
sudo iptables -A INPUT -p tcp --dport 22 -m recent --set --name SSH
sudo iptables -A INPUT -p tcp --dport 22 -m recent --update --seconds 60 --hitcount 3 --name SSH -j DROP
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT 

```

## 性能考虑

```sh
      # Use connection tracking for better performance
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Place frequently matched rules first
sudo iptables -I INPUT 1 -m state --state ESTABLISHED,RELATED -j ACCEPT

# Use specific matches to reduce processing
sudo iptables -A INPUT -p tcp --dport 80 -s 192.168.1.0/24 -j ACCEPT 

```

## 重要注意事项

+   在使规则永久化之前始终测试规则

+   保持一种方法，以防规则阻止你访问系统

+   使用特定的协议和端口而不是泛洪规则

+   监控日志以了解流量模式

+   记录你的防火墙规则以供将来参考

+   定期备份工作配置

+   考虑使用 UFW 以简化防火墙管理

`iptables` 命令提供了全面的防火墙功能，但需要仔细规划和测试，以避免安全问题和系统锁定。

更多详细信息，请参阅手册：`man iptables`
