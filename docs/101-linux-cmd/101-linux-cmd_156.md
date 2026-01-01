# `dmesg` 命令

`dmesg` 命令显示内核环形缓冲区中的消息。它显示引导消息、硬件检测、驱动加载和系统事件。这对于解决硬件问题、驱动程序问题和理解系统启动过程至关重要。

## 语法

```sh
      dmesg [options] 

```

## 关键特性

+   **内核消息**: 显示内核环形缓冲区内容

+   **引导信息**: 硬件检测和驱动加载

+   **实时监控**: 可以跟踪新消息

+   **过滤选项**: 通过设施、级别或时间过滤

+   **多种格式**: 人类可读和原始格式

## 基本用法

### 显示所有消息

```sh
      # Display all kernel messages
dmesg

# Display with human-readable timestamps
dmesg -T

# Display last 20 lines
dmesg | tail -20

# Display first 20 lines (boot messages)
dmesg | head -20 

```

### 跟踪新消息

```sh
      # Follow new kernel messages (like tail -f)
dmesg -w

# Follow with timestamps
dmesg -T -w

# Follow last 10 lines and continue
dmesg | tail -10 && dmesg -w 

```

## 消息级别和设施

### 消息级别

```sh
      # Emergency messages (level 0)
dmesg -l emerg

# Alert messages (level 1)
dmesg -l alert

# Critical messages (level 2)
dmesg -l crit

# Error messages (level 3)
dmesg -l err

# Warning messages (level 4)
dmesg -l warn

# Notice messages (level 5)
dmesg -l notice

# Info messages (level 6)
dmesg -l info

# Debug messages (level 7)
dmesg -l debug 

```

### 多级

```sh
      # Show errors and warnings
dmesg -l err,warn

# Show critical and above (crit, alert, emerg)
dmesg -l crit+

# Show warnings and below (warn, notice, info, debug)
dmesg -l warn- 

```

### 设施

```sh
      # Kernel messages
dmesg -f kern

# User-space messages
dmesg -f user

# Mail system messages
dmesg -f mail

# System daemons
dmesg -f daemon

# Authorization messages
dmesg -f auth

# Syslog messages
dmesg -f syslog 

```

## 常见选项

### 显示选项

```sh
      # -T: Human-readable timestamps
dmesg -T

# -t: Don't show timestamps
dmesg -t

# -x: Decode facility and level to human-readable
dmesg -x

# -H: Enable human-readable output
dmesg -H

# -k: Show kernel messages only
dmesg -k 

```

### 缓冲区选项

```sh
      # -c: Clear ring buffer after reading
sudo dmesg -c

# -C: Clear ring buffer
sudo dmesg -C

# -s: Buffer size
dmesg -s 65536

# -n: Set console log level
sudo dmesg -n 1 

```

## 实际示例

### 1. 硬件检测

```sh
      # Check USB device detection
dmesg | grep -i usb

# Check network interface detection
dmesg | grep -i eth

# Check disk detection
dmesg | grep -i -E "(sda|sdb|sdc|nvme)"

# Check memory detection
dmesg | grep -i memory

# Check CPU detection
dmesg | grep -i cpu 

```

### 2. 驱动加载

```sh
      # Check loaded drivers
dmesg | grep -i driver

# Check specific driver (e.g., nvidia)
dmesg | grep -i nvidia

# Check wireless driver
dmesg | grep -i wifi

# Check bluetooth driver
dmesg | grep -i bluetooth

# Check sound driver
dmesg | grep -i audio 

```

### 3. 错误调试

```sh
      # Show only error messages
dmesg -l err

# Show errors and warnings
dmesg -l err,warn

# Search for specific errors
dmesg | grep -i error

# Search for failed operations
dmesg | grep -i fail

# Search for timeout issues
dmesg | grep -i timeout 

```

### 4. 引导过程分析

```sh
      # Show boot messages with timestamps
dmesg -T | head -50

# Find boot completion time
dmesg | grep -i "boot"

# Check service startup
dmesg | grep -i systemd

# Check filesystem mounting
dmesg | grep -i mount

# Check network initialization
dmesg | grep -i network 

```

## 基于时间的过滤

### 最近消息

```sh
      # Messages from last 10 minutes
dmesg --since="10 minutes ago"

# Messages from last hour
dmesg --since="1 hour ago"

# Messages from today
dmesg --since="today"

# Messages from specific time
dmesg --since="2023-01-01 00:00:00" 

```

### 时间范围

```sh
      # Messages between specific times
dmesg --since="2023-01-01" --until="2023-01-02"

# Messages from last boot
dmesg --since="last boot"

# Messages from specific duration
dmesg --since="30 minutes ago" --until="10 minutes ago" 

```

## 高级过滤

### 1. 过滤器组合

```sh
      # Errors from last hour
dmesg -l err --since="1 hour ago"

# Kernel warnings with timestamps
dmesg -T -l warn -f kern

# USB-related errors
dmesg -l err | grep -i usb

# Network errors from today
dmesg --since="today" | grep -i -E "(network|eth|wifi)" 

```

### 2. 输出格式化

```sh
      # JSON format
dmesg --json

# Raw format (no timestamp processing)
dmesg -r

# Colored output (if supported)
dmesg --color=always

# No pager
dmesg --nopager 

```

### 3. 自定义格式化

```sh
      # Extract specific informationextract_usb_devices() {
    dmesg | grep -E "usb.*: New USB device" | \
    sed -n 's/.*New USB device found, idVendor=\([^,]*\), idProduct=\([^ ]*\).*/Vendor: \1, Product: \2/p'
}

extract_usb_devices 

```

## 监控和警报

### 1. 实时监控

```sh
      # Monitor for specific eventsmonitor_errors() {
    dmesg -w -l err | while read line; do
        echo "$(date): $line"
        # Send alert
        echo "$line" | mail -s "Kernel Error Alert" admin@domain.com
    done
}

# Monitor USB connections
monitor_usb() {
    dmesg -w | grep --line-buffered -i usb | while read line; do
        echo "USB Event: $line"
    done
} 

```

### 2. 日志分析脚本

```sh
      #!/bin/bash# Analyze kernel messages for issuesanalyze_dmesg() {
    echo "=== Kernel Message Analysis ==="
    echo "Generated: $(date)"
    echo

    echo "Error Messages:"
    dmesg -l err | tail -10
    echo

    echo "Warning Messages:"
    dmesg -l warn | tail -10
    echo

    echo "Recent Hardware Events:"
    dmesg --since="1 hour ago" | grep -i -E "(usb|disk|network|memory)" | tail -10
    echo

    echo "Driver Loading Issues:"
    dmesg | grep -i -E "(failed|error|timeout)" | grep -i driver | tail -5
}

analyze_dmesg > kernel_analysis.txt 

```

### 3. 系统健康检查

```sh
      #!/bin/bash# Check system health using dmesgcheck_system_health() {
    local issues=0

    echo "=== System Health Check ==="

    # Check for critical errors
    critical_errors=$(dmesg -l crit,alert,emerg --since="24 hours ago" | wc -l)
    if [ $critical_errors -gt 0 ]; then
        echo "⚠️  $critical_errors critical errors found in last 24 hours"
        ((issues++))
    else
        echo "✅ No critical errors in last 24 hours"
    fi

    # Check for hardware errors
    hw_errors=$(dmesg --since="24 hours ago" | grep -i -c -E "(hardware error|machine check|MCE)")
    if [ $hw_errors -gt 0 ]; then
        echo "⚠️  $hw_errors hardware errors detected"
        ((issues++))
    else
        echo "✅ No hardware errors detected"
    fi

    # Check for out of memory
    oom_events=$(dmesg --since="24 hours ago" | grep -i -c "out of memory")
    if [ $oom_events -gt 0 ]; then
        echo "⚠️  $oom_events out of memory events"
        ((issues++))
    else
        echo "✅ No out of memory events"
    fi

    # Check for filesystem errors
    fs_errors=$(dmesg --since="24 hours ago" | grep -i -c -E "(filesystem error|ext[234] error|xfs error)")
    if [ $fs_errors -gt 0 ]; then
        echo "⚠️  $fs_errors filesystem errors"
        ((issues++))
    else
        echo "✅ No filesystem errors"
    fi

    echo
    if [ $issues -eq 0 ]; then
        echo "🎉 System appears healthy!"
    else
        echo "⚠️  Found $issues types of issues - check details above"
    fi
}

check_system_health 

```

## 常见用例

### 1. USB 设备故障排除

```sh
      # Monitor USB device connectionsmonitor_usb_debug() {
    echo "Monitoring USB events (Ctrl+C to stop)..."
    dmesg -w | grep --line-buffered -i usb | while read line; do
        timestamp=$(date '+%Y-%m-%d %H:%M:%S')
        echo "[$timestamp] $line"
    done
}

# Check USB device history
usb_device_history() {
    echo "USB Device Connection History:"
    dmesg | grep -i "usb.*: New USB device" | tail -10
    echo
    echo "USB Device Disconnections:"
    dmesg | grep -i "usb.*disconnect" | tail -10
} 

```

### 2. 网络接口问题

```sh
      # Check network interface eventscheck_network_issues() {
    echo "Network Interface Events:"
    dmesg | grep -i -E "(eth[0-9]|wlan[0-9]|enp|wlp)" | tail -10
    echo

    echo "Network Driver Issues:"
    dmesg | grep -i -E "network.*error|ethernet.*error" | tail -5
    echo

    echo "Link Status Changes:"
    dmesg | grep -i "link" | tail -10
} 

```

### 3. 存储设备监控

```sh
      # Check disk health and errorscheck_storage_health() {
    echo "Storage Device Detection:"
    dmesg | grep -i -E "(sda|sdb|nvme)" | grep -i "sectors" | tail -5
    echo

    echo "Storage Errors:"
    dmesg | grep -i -E "(I/O error|Medium Error|critical medium error)" | tail -5
    echo

    echo "Filesystem Events:"
    dmesg | grep -i -E "(mounted|unmounted|remounted)" | tail -10
} 

```

## 与其他工具集成

### 1. 结合 journalctl

```sh
      # Compare kernel messagesecho "=== dmesg output ==="
dmesg --since="1 hour ago" | head -5

echo "=== journalctl kernel messages ==="
journalctl -k --since="1 hour ago" | head -5 

```

### 2. 日志文件集成

```sh
      # Save dmesg to file with rotationsave_dmesg_log() {
    local logfile="/var/log/dmesg.log"
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')

    echo "[$timestamp] === dmesg snapshot ===" >> "$logfile"
    dmesg -T >> "$logfile"

    # Rotate if file gets too large
    if [ $(stat -f%z "$logfile" 2>/dev/null || stat -c%s "$logfile") -gt 10485760 ]; then
        mv "$logfile" "$logfile.old"
        echo "Log rotated at $timestamp" > "$logfile"
    fi
} 

```

### 3. 监控系统集成

```sh
      # Create metrics for monitoring systemscreate_dmesg_metrics() {
    local errors=$(dmesg -l err --since="1 hour ago" | wc -l)
    local warnings=$(dmesg -l warn --since="1 hour ago" | wc -l)
    local timestamp=$(date +%s)

    # Output in Prometheus format
    echo "kernel_errors_total $errors $timestamp"
    echo "kernel_warnings_total $warnings $timestamp"
} 

```

## 故障排除

### 1. 缓冲区大小问题

```sh
      # Check current buffer size
dmesg | wc -l

# Increase buffer size (temporary)
sudo dmesg -s 1048576

# Make permanent (add to kernel parameters)
# Add to GRUB: log_buf_len=1048576 

```

### 2. 权限问题

```sh
      # Check if non-root can read dmesg
dmesg >/dev/null 2>&1 && echo "Can read dmesg" || echo "Cannot read dmesg"

# Check kernel parameter
cat /proc/sys/kernel/dmesg_restrict

# Allow non-root users (temporary)
sudo sysctl kernel.dmesg_restrict=0

# Make permanent
echo "kernel.dmesg_restrict = 0" | sudo tee -a /etc/sysctl.conf 

```

### 3. 消息过滤问题

```sh
      # Check available facilities and levels
dmesg --help | grep -A 20 "supported facilities"

# Test filtering
dmesg -l err | head -5
dmesg -f kern | head -5

# Check if systemd is interfering
systemctl status systemd-journald 

```

## 性能考虑

### 1. 大缓冲区处理

```sh
      # Process large buffers efficientlyprocess_large_dmesg() {
    # Use streaming instead of loading all into memory
    dmesg | while IFS= read -r line; do
        # Process line by line
        echo "$line" | grep -q "error" && echo "Error: $line"
    done
} 

```

### 2. 高效搜索

```sh
      # Use specific filters instead of post-processing
dmesg -l err               # Better than dmesg | grep -i error
dmesg --since="1 hour ago" # Better than dmesg | filtering by time 

```

## 最佳实践

### 1. 定期监控

```sh
      # Create cron job for regular checks# 0 */4 * * * /usr/local/bin/check_dmesg_errors.sh# Create check script#!/bin/bash
errors=$(dmesg -l err --since="4 hours ago" | wc -l)
if [ $errors -gt 0 ]; then
    dmesg -l err --since="4 hours ago" | mail -s "Kernel Errors Detected" admin@domain.com
fi 

```

### 2. 日志保留

```sh
      # Save important messagessave_important_messages() {
    local date_str=$(date '+%Y%m%d')
    dmesg -l err,crit,alert,emerg > "/var/log/kernel_errors_$date_str.log"
} 

```

### 3. 文档

```sh
      # Document system eventsdocument_system_event() {
    local event="$1"
    local logfile="/var/log/system_events.log"

    echo "$(date): $event" >> "$logfile"
    echo "=== dmesg at time of event ===" >> "$logfile"
    dmesg -T | tail -20 >> "$logfile"
    echo "=================================" >> "$logfile"
} 

```

## 重要注意事项

+   **root 权限**: 一些发行版将 dmesg 限制为 root 用户

+   **缓冲区大小**: 环形缓冲区大小有限；旧消息被覆盖

+   **时间戳**: 使用 `-T` 以人类可读的格式显示

+   **级别**: 理解消息级别以进行有效过滤

+   **实时**: 使用 `-w` 监控新消息

+   **性能**: 大缓冲区可能影响性能

+   **安全**: 谨慎暴露内核消息

`dmesg` 命令对于系统故障排除、硬件调试和理解内核行为至关重要。

更多详细信息，请查看手册：`man dmesg`
