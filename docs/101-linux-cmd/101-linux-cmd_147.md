# `traceroute` 命令

`traceroute` 命令用于跟踪数据包从您的计算机到网络中目标主机的路径。它显示了路径上的每个跳（路由器）并测量到达每个跳的时间。

## 语法

```sh
      traceroute [options] destination 

```

## 安装

```sh
      # Ubuntu/Debian
sudo apt update && sudo apt install traceroute

# CentOS/RHEL/Fedora
sudo yum install traceroute
# or
sudo dnf install traceroute

# macOS (usually pre-installed)
traceroute

# Check if installed
which traceroute 

```

## 基本用法

1.  **跟踪路由到网站**

```sh
      traceroute google.com
traceroute github.com
traceroute 8.8.8.8 

```

1.  **跟踪路由到 IP 地址**

```sh
      traceroute 192.168.1.1
traceroute 208.67.222.222 

```

## 选项

一些流行的选项标志包括：

```sh
      -n          Don't resolve hostnames (show IP addresses only)
-w [sec]    Set timeout for responses (default 5 seconds)
-q [num]    Set number of probe packets per hop (default 3)
-m [hops]   Set maximum number of hops (default 30)
-p [port]   Set destination port (default 33434)
-f [ttl]    Set first TTL value (starting hop)
-g [addr]   Use loose source route gateway
-I          Use ICMP ECHO instead of UDP
-T          Use TCP SYN instead of UDP
-U          Use UDP (default)
-4          Force IPv4
-6          Force IPv6
-s [addr]   Set source address
-i [iface]  Set network interface 

```

## 示例

1.  **基本跟踪路由**

```sh
      traceroute google.com 

```

1.  **仅显示 IP 地址（不进行 DNS 解析）**

```sh
      traceroute -n google.com 

```

1.  **设置自定义超时**

```sh
      traceroute -w 10 google.com 

```

1.  **使用 ICMP 而不是 UDP**

```sh
      traceroute -I google.com 

```

1.  **使用 TCP 跟踪路由**

```sh
      traceroute -T google.com 

```

1.  **设置最大跳数**

```sh
      traceroute -m 15 google.com 

```

1.  **设置每个跳数的探测次数**

```sh
      traceroute -q 1 google.com 

```

1.  **强制使用 IPv6**

```sh
      traceroute -6 ipv6.google.com 

```

1.  **设置自定义端口**

```sh
      traceroute -p 80 google.com 

```

1.  **从特定的 TTL 开始**

```sh
      traceroute -f 5 google.com 

```

## 理解输出

跟踪路由输出示例：

```sh
      traceroute to google.com (172.217.164.174), 30 hops max, 60 byte packets
 1  router.local (192.168.1.1)  1.234 ms  1.123 ms  1.045 ms
 2  10.0.0.1 (10.0.0.1)  12.345 ms  11.234 ms  10.123 ms
 3  isp-gateway.net (203.0.113.1)  25.678 ms  24.567 ms  23.456 ms
 4  * * *
 5  google-router.net (172.217.164.174)  45.123 ms  44.234 ms  43.345 ms 

```

### 输出说明：

+   **跳数**：每个路由器的顺序编号

+   **主机名/IP**：路由器的名称和 IP 地址

+   **三次**：三个探测数据包的往返时间

+   *****：表示超时或过滤响应

## 常见用例

### 1. 网络故障排除

```sh
      # Check where packets are being dropped
traceroute -n problematic-server.com

# Compare paths to different destinations
traceroute server1.com
traceroute server2.com 

```

### 2. 性能分析

```sh
      # Identify slow hops
traceroute -w 10 slow-website.com

# Check latency to different regions
traceroute eu-server.com
traceroute us-server.com
traceroute asia-server.com 

```

### 3. 网络安全分析

```sh
      # Check if traffic goes through unexpected countries
traceroute -n suspicious-site.com

# Verify VPN routing
traceroute -n whatismyip.com 

```

### 4. ISP 路由分析

```sh
      # Check ISP routing decisions
traceroute -n 8.8.8.8
traceroute -n 1.1.1.1
traceroute -n 208.67.222.222 

```

## 高级技巧

### 1. TCP 跟踪路由（tcptraceroute）

```sh
      # Install tcptraceroute
sudo apt install tcptraceroute

# Trace TCP path to web server
sudo tcptraceroute google.com 80
sudo tcptraceroute -n github.com 443 

```

### 2. MTR（我的跟踪路由）

```sh
      # Install mtr
sudo apt install mtr

# Continuous traceroute with statistics
mtr google.com
mtr -n google.com    # No DNS resolution
mtr -r google.com    # Report mode 

```

### 3. 巴黎跟踪路由

```sh
      # More accurate for load-balanced networks
sudo apt install paris-traceroute
paris-traceroute google.com 

```

## 跟踪路由变体

### 1. IPv6 跟踪路由

```sh
      # IPv6 traceroute
traceroute6 ipv6.google.com
traceroute -6 ipv6.google.com 

```

### 2. 可视化跟踪路由工具

```sh
      # Web-based visual traceroute# Visit: traceroute-online.com# or use: mtr with GUI# Install mtr-gtk for GUI
sudo apt install mtr-gtk
mtr-gtk 

```

## 分析结果

### 1. 识别问题

```sh
      # High latency at specific hop
        # Look for sudden jumps in response times
        # Packet loss
        # Look for * * * responses
        # Asymmetric routing
        # Different paths for different packets 

```

### 2. 地理分析

```sh
      # Use whois to identify hop locations
whois 203.0.113.1

# Use online IP geolocation services
# to map the route geographically 

```

## 故障排除常见问题

### 1. 超时和星号

```sh
      # Try different protocols
traceroute -I google.com    # ICMP
traceroute -T google.com    # TCP
traceroute -U google.com    # UDP (default)

# Increase timeout
traceroute -w 10 google.com 

```

### 2. 权限问题

```sh
      # UDP traceroute might need privileges
sudo traceroute google.com

# ICMP definitely needs privileges
sudo traceroute -I google.com 

```

### 3. 防火墙干扰

```sh
      # Some firewalls block traceroute# Try different ports
traceroute -p 53 google.com   # DNS port
traceroute -p 80 google.com   # HTTP port 

```

## 安全考虑

### 1. 信息披露

```sh
      # Traceroute reveals network topology# Be careful when sharing results publicly# Use -n to avoid revealing internal hostnames
traceroute -n destination 

```

### 2. 防火墙规避

```sh
      # Try different protocols if blocked
traceroute -T -p 443 target.com
traceroute -I target.com 

```

## 自动化和脚本

### 1. 批量跟踪路由

```sh
      #!/bin/bash# Trace routes to multiple destinations
destinations=("google.com" "github.com" "stackoverflow.com")

for dest in "${destinations[@]}"; do
    echo "Tracing route to $dest"
    traceroute -n "$dest" > "traceroute_$dest.txt"
done 

```

### 2. 监控脚本

```sh
      #!/bin/bash# Monitor route changeswhile true; do
    traceroute -n google.com > "/tmp/trace_$(date +%s).txt"
    sleep 3600  # Check every hour
done 

```

### 3. 路由比较

```sh
      #!/bin/bash# Compare routes from different locationsecho "Route from current location:"
traceroute -n $1

echo "Route from VPN:"
# Connect to VPN and run again 

```

## 替代命令

### 1. pathping（Windows 等效）

```sh
      # On Windows systems
pathping google.com 

```

### 2. mtr（更好的替代品）

```sh
      # Continuous monitoring
mtr --report google.com
mtr --report-cycles 10 google.com 

```

### 3. hping3（高级探测）

```sh
      sudo apt install hping3
sudo hping3 -T -p 80 -c 3 google.com 

```

## 性能优化

### 1. 更快的跟踪路由

```sh
      # Reduce probes per hop
traceroute -q 1 google.com

# Reduce max hops
traceroute -m 15 google.com

# Skip DNS resolution
traceroute -n google.com 

```

### 2. 详细分析

```sh
      # More probes for accuracy
traceroute -q 5 google.com

# Longer timeout for slow links
traceroute -w 15 google.com 

```

## 重要注意事项

+   跟踪路由可能无法显示负载均衡网络中的实际路径

+   一些路由器不会对跟踪路由探测做出响应

+   由于路由变化，运行结果可能不同

+   ICMP 跟踪路由通常比 UDP 更有效

+   现代网络可能使用 ECMP（等成本多路径）路由

+   VPN 和代理会改变明显的路由

`traceroute` 命令对于网络诊断至关重要，有助于识别路由问题、网络性能问题和理解网络拓扑。

想要更多细节，请查看手册：`man traceroute`
