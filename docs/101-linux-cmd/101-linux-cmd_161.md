# ifplugstatus 命令

`ifplugstatus` 命令是一个诊断工具，用于检查 Linux 系统上网络接口的**物理链路状态**。它报告接口（如 eth0、enp0s3 或 wlan0）是否有网络电缆连接。此工具特别适用于故障排除有线网络连接和检测未插拔的电缆。

## 语法

```sh
      ifplugstatus [options] [interface] 

```

### 参数

+   interface — 要检查的网络接口（例如，eth0、enp0s3、wlan0）。

+   选项 — 用于自定义输出的可选标志。

## 安装

`ifplugstatus` 是 `ifplugd` 软件包的一部分。您可以使用以下命令根据您的发行版进行安装：

```sh
      # Ubuntu/Debian
sudo apt update
sudo apt install ifplugd

# CentOS/RHEL/Fedora
sudo yum install ifplugd
# or
sudo dnf install ifplugd 

```

### 要验证安装：

```sh
      which ifplugstatus 

```

## 基本用法

### 1. 检查单个接口的状态

```sh
      ifplugstatus eth0 

```

### 样本输出：

```sh
      eth0: link beat detected 

```

### 或

```sh
      eth0: link beat not detected 

```

### 说明：

+   链路检测到链接 → 电缆已插入并激活。

+   链路检测未检测到 → 电缆未插拔或未激活。*注意：'链路检测'是旧式以太网（10/100 Mbps）的术语。现代千兆位或更高接口可能使用不同的链路检测信号，但命令通常报告一个活动的物理链路。*

### 2. 检查多个接口

```sh
      ifplugstatus eth0 wlan0 

```

### 样本输出：

```sh
      eth0: link beat detected
wlan0: link beat not detected 

```

这允许快速验证所有可用接口。

## 选项

一些流行的选项标志包括：

| 选项 | 描述 |
| --- | --- |
| `-a` | 显示所有接口（默认行为）。 |
| `-i` | 指定特定接口。 |
| `-s` | 仅显示简短摘要。 |
| `-v` | 详细输出，包含更多详细信息。 |
| `-q` | 静默模式 — 最小输出。 |
| `-h` | 显示帮助信息。 |

## 实际示例

### 1. 检查主以太网接口是否连接

```sh
      ifplugstatus eth0 

```

显示 eth0 的当前电缆连接状态。

### 2. 检查所有可用网络接口

```sh
      ifplugstatus -a 

```

列出每个检测到的接口的链路状态。

### 3. 使用详细模式以获取详细信息

```sh
      ifplugstatus -v eth0 

```

提供有关链路状态的更多描述性信息。

### 4. 脚本或自动化模式的静默模式

```sh
      ifplugstatus -q eth0 

```

仅输出必要信息，适用于 shell 脚本。

## 理解输出

典型输出可能如下所示：

```sh
      eth0: link beat detected
enp0s3: link beat not detected 

```

+   接口名称：eth0、enp0s3 等。

+   链路状态：指示是否存在物理链路（电缆或信号）。

当集成到脚本中时，这有助于自动化检测断开连接的接口或故障布线。

## 常见用例

### 1. 电缆连接测试

```sh
      ifplugstatus eth0 

```

快速验证电缆是否物理连接到网络端口。

### 2. 多接口监控

```sh
      ifplugstatus eth0 wlan0 

```

同时检查多个接口的链路活动。

### 3. 持续监控

```sh
      watch -n 5 ifplugstatus eth0 

```

每隔 5 秒更新接口状态，以进行实时链路监控。

## 常见问题故障排除

### 1. 接口未找到

如果提供了错误的接口名称：

```sh
      eth5: No such device 

```

**修复方法：** 使用 ip a 或 ifconfig 命令列出有效接口。

### 2. 权限拒绝

一些系统可能需要提升权限：

```sh
      sudo ifplugstatus eth0 

```

### 3. 命令未找到

如果命令缺失：

```sh
      ifplugstatus: command not found 

```

**修复方法：** 使用您的包管理器安装 ifplugd 软件包。

**提示**：始终确保已安装 ifplugd 软件包，并且您的用户有权访问网络接口。

## 相关命令

| 命令 | 描述 |
| --- | --- |
| `ip a`（推荐）/ `ifconfig` | 显示所有网络接口及其详细信息。`ip a`是现代工具；`ifconfig`是旧版工具。 |
| `ethtool eth0` | 获取关于网络接口的高级信息。 |
| `nmcli device status` | 通过 NetworkManager 检查连接状态。 |
| `ping` | 在确认电缆连接后测试网络可达性。 |

## 重要注意事项

+   与有线以太网接口配合使用最佳。

+   无线接口可能不会总是准确报告链路状态。

+   工具为只读模式 — 它不会修改系统设置。

+   轻量级且适用于桌面和服务器环境的安全使用。

## 手册参考

对于更多信息和高阶使用：

```sh
      man ifplugstatus 

```
