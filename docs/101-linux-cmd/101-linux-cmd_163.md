# `nmtui` 命令

**`nmtui`**（网络管理器文本用户界面）命令是一个用于在 Linux 中配置网络连接的 **菜单驱动工具**。

它提供了一个简单、基于文本的界面来管理网络设置，如 Wi-Fi、以太网、主机名等，而无需使用复杂的命令行选项。

* * *

### 🔹 它的作用

`nmtui` 允许您：

+   查看和编辑 **网络连接**

+   **激活或停用** 接口

+   **设置或更改**系统 **主机名**

+   连接到 **Wi-Fi 网络**

+   管理 **IPv4 / IPv6 设置**

它在服务器或 **没有图形桌面环境** 的系统上特别有用。

* * *

### 🧠 语法

nmtui [选项]

* * *

### 🖥️ 示例

1.  **打开主菜单界面：**

    nmtui

    打开主 TUI（文本用户界面）窗口，其中包含 *编辑连接*、*激活连接* 或 *设置系统主机名* 的选项。

1.  **直接编辑网络连接：**

    nmtui edit

    允许您创建、修改或删除网络连接（包括以太网和 Wi-Fi）。

1.  **直接激活或停用连接：**

    nmtui connect

    打开 *激活连接* 菜单，您可以在其中启用或禁用网络接口。

1.  **设置或更改系统主机名：**

    nmtui hostname

    打开一个对话框来设置您的系统主机名（用于网络上的识别）。

1.  **直接连接到特定的 Wi-Fi 网络：**

    nmtui connect "MyWiFiNetwork"

    如果范围内存在，则连接到指定的 Wi-Fi 网络。

1.  **退出界面：**

    简单地使用 **Tab → 退出** 或按 **Esc**。

* * *

### 🧩 常用选项和子命令

| **命令** | **描述** |
| :-- | :-- |
| `nmtui` | 启动交互式主菜单 |
| `nmtui edit` | 打开“编辑连接”屏幕 |
| `nmtui connect` | 打开“激活连接”屏幕 |
| `nmtui hostname` | 打开“设置系统主机名”对话框 |
| `nmtui connect <SSID>` | 直接连接到特定的 Wi-Fi 网络 |
| `nmtui help` | 显示基本使用帮助 |

* * *

### ⚙️ 接口导航

+   使用 **箭头键** 或 **Tab** 在字段之间移动。

+   按下 **Enter** 键进行选择。

+   使用 **空格键** 切换复选框或选项。

+   按下 **Esc** 或选择 **退出** 以安全退出。

* * *

### 📡 常见用例

**1. 配置静态 IP 地址：**

nmtui edit

→ 选择您的以太网连接

→ 将“IPv4 配置”设置为 *手动* 

→ 输入 IP、网关和 DNS

→ 保存 → 激活连接

* * *

**2. 从终端连接 Wi-Fi：**

nmtui connect

→ 选择您的无线接口

→ 选择 SSID

→ 输入密码

→ 激活连接

* * *

**3. 更改主机名：**

nmtui hostname

→ 输入您的新主机名

→ 确认并退出

→ 新的主机名将立即生效。

* * *

### 🛠 故障排除

| **问题** | **可能的解决方案** |
| :-- | :-- |
| `nmtui` 命令未找到 | 安装 NetworkManager TUI：`sudo apt install network-manager`（Debian/Ubuntu）或 `sudo dnf install NetworkManager-tui`（Fedora/RHEL/CentOS） |
| 变更不会生效 | 重新启动 NetworkManager：`sudo systemctl restart NetworkManager` |
| 接口未显示 | 确保您的接口由 NetworkManager 管理：检查 `/etc/NetworkManager/NetworkManager.conf` |
| Wi-Fi 缺失 | 确保已安装无线驱动程序，并且 `nmcli device status` 显示 Wi-Fi 网卡 |

* * *

### 🧰 相关命令

| **命令** | **用途** |
| :-- | :-- |
| `nmcli` | 命令行（非交互式）NetworkManager 控制 |
| `ip addr` | 显示所有接口的 IP 地址 |
| `ifconfig` | （已弃用）接口配置命令 |
| `systemctl restart NetworkManager` | 重新启动 NetworkManager 服务 |
| `hostnamectl` | 直接管理系统主机名 |

* * *

### 🧾 示例：通过 `nmtui` 配置以太网

sudo nmtui

→ 选择 **编辑连接**

→ 选择 **有线连接 1**

→ 将 IPv4 设置为 **手动**

→ 填写：地址：192.168.1.50/24

网关：192.168.1.1

DNS：8.8.8.8

→ 保存 → 返回 → **激活连接** → 选择 **有线连接 1**

✅ 您的网络连接现在将使用静态 IP。

* * *

### 📘 备注

+   使用 `nmtui` 做的更改在重启后依然有效。

+   需要 **NetworkManager** 激活。

+   `nmtui` 提供了与 `nmcli` 相同的功能，但界面更加用户友好。

+   非常适合 **服务器环境** 或 **SSH 会话**，在这些环境中 GUI 工具不可用。
