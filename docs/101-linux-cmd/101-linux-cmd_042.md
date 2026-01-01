# `ip`命令

`ip`命令是 iproute2 包中的一个强大工具，用于网络管理任务。它作为较老网络工具（如`ifconfig`、`route`和`arp`）的现代替代品。`ip`命令可以显示或操作路由、网络设备、接口和隧道。

## 语法

```sh
      ip [ OPTIONS ] OBJECT { COMMAND | help } 

```

## 关键功能

+   **接口管理**：配置和监控网络接口

+   **IP 地址管理**：添加、删除和显示 IP 地址

+   **路由控制**：管理路由表和路由

+   **邻居管理**：处理 ARP/邻居缓存条目

+   **网络命名空间**：使用网络命名空间

+   **隧道**：创建和管理网络隧道

## 基本用法

### 显示网络接口

```sh
      # Show all network interfaces
ip link show

# Show specific interface
ip link show eth0

# Show interface statistics
ip -s link show eth0 

```

### IP 地址管理

```sh
      # Show all IP addresses
ip addr show

# Show addresses for specific interface
ip addr show eth0

# Add IP address to interface
sudo ip addr add 192.168.1.100/24 dev eth0

# Remove IP address from interface
sudo ip addr del 192.168.1.100/24 dev eth0

# Flush all addresses from interface
sudo ip addr flush dev eth0 

```

## 接口管理

### 接口启用/禁用

```sh
      # Bring interface up
sudo ip link set eth0 up

# Bring interface down
sudo ip link set eth0 down

# Set interface MTU
sudo ip link set eth0 mtu 1400

# Change MAC address
sudo ip link set eth0 address 00:11:22:33:44:55 

```

### 创建虚拟接口

```sh
      # Create VLAN interface
sudo ip link add link eth0 name eth0.100 type vlan id 100

# Create bridge interface
sudo ip link add name br0 type bridge

# Create virtual ethernet pair
sudo ip link add veth0 type veth peer name veth1

# Delete virtual interface
sudo ip link delete veth0 

```

## 路由管理

### 查看路由

```sh
      # Show routing table
ip route show

# Show routes for specific destination
ip route get 8.8.8.8

# Show routes via specific interface
ip route show dev eth0

# Show IPv6 routes
ip -6 route show 

```

### 管理路由

```sh
      # Add default route
sudo ip route add default via 192.168.1.1

# Add specific route
sudo ip route add 10.0.0.0/8 via 192.168.1.1

# Add route via specific interface
sudo ip route add 172.16.0.0/16 dev eth1

# Delete route
sudo ip route del 10.0.0.0/8

# Replace existing route
sudo ip route replace default via 192.168.1.254 

```

### 多重路由表

```sh
      # Show all routing tables
ip route show table all

# Add route to specific table
sudo ip route add 192.168.2.0/24 via 10.0.0.1 table 100

# Show specific routing table
ip route show table 100

# Add routing rule
sudo ip rule add from 192.168.1.0/24 table 100 

```

## 邻居（ARP）管理

### ARP 缓存操作

```sh
      # Show ARP cache
ip neigh show

# Show neighbors for specific interface
ip neigh show dev eth0

# Add static ARP entry
sudo ip neigh add 192.168.1.50 lladdr 00:11:22:33:44:55 dev eth0

# Delete ARP entry
sudo ip neigh del 192.168.1.50 dev eth0

# Flush ARP cache
sudo ip neigh flush all 

```

## 高级功能

### 网络命名空间

```sh
      # List network namespaces
ip netns list

# Create network namespace
sudo ip netns add myns

# Execute command in namespace
sudo ip netns exec myns ip addr show

# Delete network namespace
sudo ip netns del myns

# Move interface to namespace
sudo ip link set eth1 netns myns 

```

### 隧道

```sh
      # Create GRE tunnel
sudo ip tunnel add gre1 mode gre remote 10.0.0.2 local 10.0.0.1 ttl 255

# Create IPIP tunnel
sudo ip tunnel add ipip1 mode ipip remote 192.168.1.2 local 192.168.1.1

# Show tunnels
ip tunnel show

# Delete tunnel
sudo ip tunnel del gre1 

```

### 流量控制

```sh
      # Show queueing disciplines
ip qdisc show

# Add traffic shaping
sudo ip qdisc add dev eth0 root handle 1: htb default 30

# Show traffic control statistics
ip -s qdisc show dev eth0 

```

## 监控和统计

### 接口统计

```sh
      # Show detailed interface statistics
ip -s link show

# Show extended statistics
ip -s -s link show eth0

# Monitor interface changes
ip monitor link

# Monitor address changes
ip monitor addr 

```

### 实时监控

```sh
      # Monitor all network events
ip monitor

# Monitor only route changes
ip monitor route

# Monitor with timestamps
ip -t monitor 

```

## 常见选项

### 通用选项

```sh
      # Use specific protocol family
ip -4 addr show    # IPv4 only
ip -6 addr show    # IPv6 only

# Show more details
ip -d link show

# Output in JSON format
ip -j addr show

# Colorize output
ip -c addr show

# Don't resolve names
ip -n route show 

```

### 批量操作

```sh
      # Execute commands from file
sudo ip -batch commands.txt

# Example batch file content:
# link set eth0 up
# addr add 192.168.1.100/24 dev eth0
# route add default via 192.168.1.1 

```

## 实际示例

### 设置静态 IP

```sh
      # Complete static IP configuration
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up
sudo ip route add default via 192.168.1.1

# Add DNS (edit /etc/resolv.conf)
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf 

```

### 创建带有 VLAN 的桥接

```sh
      # Create bridge
sudo ip link add name br0 type bridge

# Create VLAN interfaces
sudo ip link add link eth0 name eth0.10 type vlan id 10
sudo ip link add link eth0 name eth0.20 type vlan id 20

# Add interfaces to bridge
sudo ip link set eth0.10 master br0
sudo ip link set eth0.20 master br0

# Bring everything up
sudo ip link set br0 up
sudo ip link set eth0.10 up
sudo ip link set eth0.20 up 

```

### 网络故障排除

```sh
      # Check connectivity path
ip route get 8.8.8.8

# Verify interface status
ip link show | grep -E "(UP|DOWN)"

# Check for duplicate IPs
ip addr show | grep inet

# Monitor network changes
ip monitor all 

```

## 选项参考

| **选项** | **描述** |
| :-- | :-- |
| `-4, -6` | 使用 IPv4 或 IPv6 协议族 |
| `-b, -batch` | 从文件中读取命令 |
| `-c, -color` | 使用彩色输出 |
| `-d, -details` | 显示详细信息 |
| `-f, -family` | 指定协议族 |
| `-h, -human` | 人类可读输出 |
| `-j, -json` | JSON 输出格式 |
| `-n, -numeric` | 不解析名称 |
| `-o, -oneline` | 单行输出 |
| `-r, -resolve` | 解析主机名 |
| `-s, -stats` | 显示统计信息 |
| `-t, -timestamp` | 显示时间戳 |

## 对象参考

| **对象** | **描述** |
| :-- | :-- |
| `link` | 网络设备（接口） |
| `addr` | IPv4 或 IPv6 地址 |
| `route` | 路由表条目 |
| `rule` | 路由策略数据库中的规则 |
| `neigh` | 邻居（ARP）表条目 |
| `ntable` | 邻居表配置 |
| `tunnel` | 在 IP 上建立隧道 |
| `maddr` | 多播地址 |
| `mroute` | 多播路由缓存条目 |
| `monitor` | 监视 netlink 消息 |

## 重要提示

+   `ip`命令在大多数配置更改时需要 root 权限

+   使用`ip`做出的更改是立即的，但重启后不会持久

+   对于持久配置，请使用网络配置文件或 NetworkManager

+   在更改配置之前，始终备份网络配置

+   使用`ip`代替过时的工具，如`ifconfig`和`route`

## 与 NetworkManager 集成

```sh
      # Check if NetworkManager is managing interface
nmcli device status

# Temporarily disable NetworkManager for interface
sudo nmcli device set eth0 managed no

# Re-enable NetworkManager management
sudo nmcli device set eth0 managed yes 

```

`ip`命令对于现代 Linux 网络管理至关重要，并提供了对网络配置的全面控制。

想要了解更多细节，请查看手册：`man ip`
