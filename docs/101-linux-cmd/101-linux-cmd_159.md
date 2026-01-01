# `lspci`命令

`lspci`命令列出系统连接的所有 PCI（外围组件互连）设备。它提供了关于硬件组件的详细信息，包括显卡、网络适配器、声卡、存储控制器以及其他基于 PCI 的设备。

## 语法

```sh
      lspci [options] 

```

## 关键特性

+   **硬件检测**：列出所有 PCI 设备

+   **详细信息**：供应商、设备、类别和能力

+   **树视图**：显示设备层次结构和关系

+   **过滤选项**：按供应商、设备或类别搜索

+   **详细输出**：多级详细信息

## 基本用法

### 简单设备列表

```sh
      # List all PCI devices
lspci

# Example output:
# 00:00.0 Host bridge: Intel Corporation Device 9b61
# 00:01.0 PCI bridge: Intel Corporation Device 1901
# 00:02.0 VGA compatible controller: Intel Corporation Device 9bc4
# 00:14.0 USB controller: Intel Corporation Device a36d 

```

### 人类可读输出

```sh
      # Show device names instead of just IDs
lspci -nn

# More verbose output
lspci -v

# Very verbose output (includes everything)
lspci -vv

# Extremely verbose output
lspci -vvv 

```

## 常见选项

### 基本选项

```sh
      # -v: Verbose (show detailed info)
lspci -v

# -vv: Very verbose
lspci -vv

# -n: Show numeric IDs instead of names
lspci -n

# -nn: Show both names and numeric IDs
lspci -nn

# -k: Show kernel drivers
lspci -k 

```

### 显示选项

```sh
      # -t: Tree format
lspci -t

# -tv: Tree format with verbose info
lspci -tv

# -x: Show hex dump of config space
lspci -x

# -xxx: Show full hex dump
lspci -xxx 

```

## 过滤和选择

### 1. 按设备类型

```sh
      # Graphics cards
lspci | grep -i vga
lspci | grep -i display

# Network adapters
lspci | grep -i network
lspci | grep -i ethernet

# USB controllers
lspci | grep -i usb

# Sound cards
lspci | grep -i audio
lspci | grep -i sound

# Storage controllers
lspci | grep -i storage
lspci | grep -i sata 

```

### 2. 按供应商

```sh
      # Intel devices
lspci | grep -i intel

# AMD devices
lspci | grep -i amd

# NVIDIA devices
lspci | grep -i nvidia

# Broadcom devices
lspci | grep -i broadcom 

```

### 3. 特定设备选择

```sh
      # Select specific device by bus ID
lspci -s 00:02.0

# Select multiple devices
lspci -s 00:02.0,00:14.0

# Select by vendor ID
lspci -d 8086:

# Select by vendor and device ID
lspci -d 8086:9bc4 

```

## 详细信息

### 1. 详细设备信息

```sh
      # Show capabilities and features
lspci -v

# Example output includes:
# - Memory addresses
# - IRQ assignments
# - Capabilities (MSI, Power Management, etc.)
# - Kernel modules/drivers

# Very detailed information
lspci -vv
# Includes:
# - Extended capabilities
# - Configuration registers
# - Link status and speeds 

```

### 2. 驱动程序信息

```sh
      # Show which kernel drivers are in use
lspci -k

# Example output:
# 00:02.0 VGA compatible controller: Intel Corporation Device 9bc4
#         Subsystem: Dell Device 097d
#         Kernel driver in use: i915
#         Kernel modules: i915

# Combine with verbose for more detail
lspci -vk 

```

### 3. 配置空间

```sh
      # Show configuration space (hex dump)
lspci -x

# Full configuration space
lspci -xxx

# Specific device configuration
lspci -s 00:02.0 -xxx 

```

## 树视图和拓扑

### 1. 设备层次结构

```sh
      # Show device tree structure
lspci -t

# Example output:
# -[0000:00]-+-00.0  Intel Corporation Device 9b61
#            +-01.0-[01]--
#            +-02.0  Intel Corporation Device 9bc4
#            +-14.0  Intel Corporation Device a36d

# Tree with device names
lspci -tv 

```

### 2. 总线信息

```sh
      # Show bridges and connections
lspci -tv | grep -E "(bridge|Bridge)"

# PCI Express information
lspci -vv | grep -A 5 -B 5 "Express"

# Link capabilities and status
lspci -vv | grep -E "(Link|Speed|Width)" 

```

## 硬件分析

### 1. 显卡信息

```sh
      # List graphics devicesget_gpu_info() {
    echo "=== Graphics Cards ==="
    lspci | grep -i -E "(vga|display|3d)"
    echo

    echo "=== Detailed GPU Information ==="
    lspci -v | grep -A 10 -B 2 -i -E "(vga|display)"
    echo

    echo "=== GPU Drivers ==="
    lspci -k | grep -A 3 -B 1 -i -E "(vga|display)"
}

get_gpu_info 

```

### 2. 网络接口分析

```sh
      # Network adapter detailsget_network_info() {
    echo "=== Network Adapters ==="
    lspci | grep -i -E "(network|ethernet|wireless)"
    echo

    echo "=== Network Driver Information ==="
    lspci -k | grep -A 3 -B 1 -i -E "(network|ethernet)"
    echo

    echo "=== Wireless Capabilities ==="
    lspci -vv | grep -A 20 -B 5 -i wireless
}

get_network_info 

```

### 3. 存储控制器信息

```sh
      # Storage device detailsget_storage_info() {
    echo "=== Storage Controllers ==="
    lspci | grep -i -E "(storage|sata|ide|scsi|nvme)"
    echo

    echo "=== Storage Driver Information ==="
    lspci -k | grep -A 3 -B 1 -i -E "(storage|sata|ahci)"
    echo

    echo "=== SATA Capabilities ==="
    lspci -vv | grep -A 10 -B 2 -i sata
}

get_storage_info 

```

## 系统分析脚本

### 1. 硬件清单

```sh
      #!/bin/bash# Complete hardware inventory using lspcihardware_inventory() {
    echo "=== PCI Hardware Inventory ==="
    echo "Generated: $(date)"
    echo

    echo "--- System Overview ---"
    lspci | wc -l | xargs echo "Total PCI devices:"
    echo

    echo "--- Graphics ---"
    lspci | grep -i -E "(vga|display|3d)" || echo "No graphics devices found"
    echo

    echo "--- Network ---"
    lspci | grep -i -E "(network|ethernet|wireless)" || echo "No network devices found"
    echo

    echo "--- Storage ---"
    lspci | grep -i -E "(storage|sata|ide|nvme)" || echo "No storage controllers found"
    echo

    echo "--- Audio ---"
    lspci | grep -i -E "(audio|sound|multimedia)" || echo "No audio devices found"
    echo

    echo "--- USB Controllers ---"
    lspci | grep -i usb || echo "No USB controllers found"
    echo

    echo "--- Other Devices ---"
    lspci | grep -v -i -E "(vga|display|3d|network|ethernet|wireless|storage|sata|ide|nvme|audio|sound|multimedia|usb|bridge|host)" || echo "No other devices"
}

hardware_inventory > hardware_inventory.txt 

```

### 2. 驱动状态检查

```sh
      #!/bin/bash# Check driver status for all PCI devicescheck_drivers() {
    echo "=== PCI Driver Status ==="

    lspci | while read line; do
        device_id=$(echo "$line" | cut -d' ' -f1)
        device_name=$(echo "$line" | cut -d' ' -f2-)

        driver_info=$(lspci -k -s "$device_id" | grep "Kernel driver in use")

        if [ -n "$driver_info" ]; then
            driver=$(echo "$driver_info" | cut -d':' -f2 | xargs)
            echo "✓ $device_id: $device_name -> $driver"
        else
            echo "✗ $device_id: $device_name -> NO DRIVER"
        fi
    done
}

check_drivers 

```

### 3. 性能分析

```sh
      #!/bin/bash# Analyze PCI device performance capabilitiesanalyze_performance() {
    echo "=== PCI Performance Analysis ==="

    echo "--- PCIe Link Speeds ---"
    lspci -vv | grep -A 1 -B 1 "Link.*Speed" | grep -E "(:|Speed|Width)"
    echo

    echo "--- Memory Mappings ---"
    lspci -v | grep -E "(Memory at|I/O ports at)" | sort | uniq -c | sort -nr
    echo

    echo "--- Power Management ---"
    lspci -vv | grep -c "Power Management" | xargs echo "Devices with power management:"
    echo

    echo "--- MSI Capabilities ---"
    lspci -vv | grep -c "MSI:" | xargs echo "Devices with MSI support:"
    echo

    echo "--- 64-bit Devices ---"
    lspci -vv | grep -c "64-bit" | xargs echo "64-bit capable devices:"
}

analyze_performance 

```

## 故障排除

### 1. 设备检测问题

```sh
      # Check if device is detectedcheck_device_detection() {
    local search_term="$1"

    echo "Searching for: $search_term"

    devices=$(lspci | grep -i "$search_term")
    if [ -n "$devices" ]; then
        echo "✓ Device(s) found:"
        echo "$devices"
        echo

        echo "Driver information:"
        echo "$devices" | while read line; do
            device_id=$(echo "$line" | cut -d' ' -f1)
            lspci -k -s "$device_id" | grep -E "(driver|module)"
        done
    else
        echo "✗ No devices found matching '$search_term'"
        echo "Try checking:"
        echo "  - Physical connections"
        echo "  - BIOS/UEFI settings"
        echo "  - Power supply"
    fi
}

# Usage examples
check_device_detection "graphics"
check_device_detection "network" 

```

### 2. 驱动程序问题

```sh
      # Find devices without driversfind_missing_drivers() {
    echo "=== Devices Without Drivers ==="

    lspci | while read line; do
        device_id=$(echo "$line" | cut -d' ' -f1)
        device_name=$(echo "$line" | cut -d' ' -f2-)

        if ! lspci -k -s "$device_id" | grep -q "Kernel driver in use"; then
            echo "Missing driver: $device_id - $device_name"

            # Try to find available modules
            modules=$(lspci -k -s "$device_id" | grep "Kernel modules:" | cut -d':' -f2)
            if [ -n "$modules" ]; then
                echo "  Available modules:$modules"
            fi
        fi
    done
}

find_missing_drivers 

```

### 3. 硬件兼容性

```sh
      # Check hardware compatibilitycheck_compatibility() {
    echo "=== Hardware Compatibility Check ==="

    echo "--- Unsupported Devices ---"
    lspci -nn | while read line; do
        if echo "$line" | grep -q "\[ffff:"; then
            echo "Possible unsupported device: $line"
        fi
    done
    echo

    echo "--- Legacy Devices ---"
    lspci | grep -i -E "(legacy|isa|parallel|serial|floppy)" || echo "No legacy devices found"
    echo

    echo "--- Vendor Support ---"
    echo "Intel devices: $(lspci | grep -i intel | wc -l)"
    echo "AMD devices: $(lspci | grep -i amd | wc -l)"
    echo "NVIDIA devices: $(lspci | grep -i nvidia | wc -l)"
    echo "Other vendors: $(lspci | grep -v -i -E "(intel|amd|nvidia)" | wc -l)"
}

check_compatibility 

```

## 高级用法

### 1. 配置空间分析

```sh
      # Analyze specific device configurationanalyze_device_config() {
    local device_id="$1"

    echo "=== Configuration Analysis for $device_id ==="

    echo "--- Basic Information ---"
    lspci -s "$device_id" -v
    echo

    echo "--- Configuration Space ---"
    lspci -s "$device_id" -x
    echo

    echo "--- Capabilities ---"
    lspci -s "$device_id" -vv | grep -A 20 "Capabilities:"
}

# Usage: analyze_device_config "00:02.0" 

```

### 2. 带宽分析

```sh
      # Analyze PCIe bandwidthanalyze_bandwidth() {
    echo "=== PCIe Bandwidth Analysis ==="

    lspci -vv | grep -A 2 -B 2 "Express.*Root Port\|Express.*Endpoint" | \
    while read line; do
        if echo "$line" | grep -q "Express"; then
            echo "Device: $line"
        elif echo "$line" | grep -q "Link.*Speed"; then
            echo "  $line"
        fi
    done
}

analyze_bandwidth 

```

### 3. 电源管理

```sh
      # Check power management capabilitiescheck_power_management() {
    echo "=== Power Management Status ==="

    lspci -vv | grep -B 5 -A 10 "Power Management" | \
    grep -E "(^[0-9a-f]{2}:[0-9a-f]{2}\.[0-9]|Power Management|PME)"
}

check_power_management 

```

## 与其他工具集成

### 1. 与 lsmod 结合

```sh
      # Match PCI devices with loaded modulesmatch_devices_modules() {
    echo "=== PCI Devices and Kernel Modules ==="

    lspci -k | grep -E "(^[0-9a-f]{2}:|Kernel driver|Kernel modules)" | \
    while read line; do
        if [[ "$line" =~ ^[0-9a-f]{2}: ]]; then
            echo "$line"
        elif [[ "$line" =~ "Kernel driver in use:" ]]; then
            driver=$(echo "$line" | cut -d':' -f2 | xargs)
            echo "  Active driver: $driver"
            lsmod | grep "^$driver" && echo "    ✓ Module loaded" || echo "    ✗ Module not found"
        fi
    done
} 

```

### 2. 与 udev 结合

```sh
      # Check udev rules for PCI devicescheck_udev_rules() {
    local device_id="$1"

    echo "Checking udev rules for device: $device_id"

    # Get vendor and device IDs
    vendor_device=$(lspci -n -s "$device_id" | awk '{print $3}')
    vendor_id=$(echo "$vendor_device" | cut -d':' -f1)
    device_id_hex=$(echo "$vendor_device" | cut -d':' -f2)

    echo "Vendor ID: $vendor_id, Device ID: $device_id_hex"

    # Search udev rules
    find /etc/udev/rules.d /lib/udev/rules.d -name "*.rules" -exec grep -l "$vendor_id\|$device_id_hex" {} \; 2>/dev/null
} 

```

## 安全考虑

### 1. 硬件安全

```sh
      # Check for security-relevant hardwarecheck_security_hardware() {
    echo "=== Security Hardware Check ==="

    echo "--- TPM Devices ---"
    lspci | grep -i tpm || echo "No TPM devices found"
    echo

    echo "--- Virtualization Support ---"
    lspci -vv | grep -i -E "(vt-x|amd-v|virtualization)" || echo "Check CPU flags for virtualization"
    echo

    echo "--- IOMMU Support ---"
    lspci -vv | grep -i iommu || echo "No explicit IOMMU references found"
    echo

    echo "--- Hardware Security Modules ---"
    lspci | grep -i -E "(security|crypto|hsm)" || echo "No HSM devices found"
}

check_security_hardware 

```

## 最佳实践

### 1. 定期硬件监控

```sh
      # Create hardware monitoring script#!/bin/bash# Monitor hardware changes

BASELINE_FILE="/var/lib/hardware_baseline.txt"
CURRENT_FILE="/tmp/current_hardware.txt"

# Generate current state
lspci -nn > "$CURRENT_FILE"

# Compare with baseline
if [ -f "$BASELINE_FILE" ]; then
    if ! diff -q "$BASELINE_FILE" "$CURRENT_FILE" >/dev/null; then
        echo "Hardware configuration changed!"
        echo "Changes:"
        diff "$BASELINE_FILE" "$CURRENT_FILE"
    fi
else
    echo "Creating hardware baseline"
fi

# Update baseline
cp "$CURRENT_FILE" "$BASELINE_FILE" 

```

### 2. 文档

```sh
      # Generate hardware documentationdocument_hardware() {
    local output_file="hardware_documentation_$(date +%Y%m%d).txt"

    {
        echo "Hardware Documentation"
        echo "Generated: $(date)"
        echo "Hostname: $(hostname)"
        echo "========================="
        echo

        echo "PCI Device Summary:"
        lspci | wc -l | xargs echo "Total devices:"
        echo

        echo "Detailed Device List:"
        lspci -nn
        echo

        echo "Driver Status:"
        lspci -k
        echo

        echo "Device Tree:"
        lspci -tv

    } > "$output_file"

    echo "Documentation saved to: $output_file"
} 

```

## 重要注意事项

+   **根访问**：一些详细信息需要 root 权限

+   **硬件检测**：仅显示连接到 PCI 总线的设备

+   **驱动状态**：显示当前加载的驱动程序，并非所有可用的驱动程序

+   **更新**：设备信息从内核读取，可能需要硬件重新扫描

+   **供应商 ID**：数字 ID 是标准化的，名称来自 PCI ID 数据库

+   **树视图**：显示物理总线拓扑和设备关系

`lspci`命令对于硬件故障排除、驱动程序管理和系统分析至关重要。

更多详细信息，请查看手册：`man lspci`
