# `nmcli` 命令

`nmcli` 命令通过控制 NetworkManager（一个处理网络的后台守护进程）来管理网络连接，该命令代表网络管理器命令行界面。

### 安装：

`nmcli` 命令默认已安装在大多数 Linux 发行版中。要检查它是否已安装在您的系统上，请输入：

```sh
      nmcli --version 

```

如果您没有安装 nmcli，您可以使用包管理器进行安装：

Ubuntu 或 Debian：

```sh
      sudo apt install network manager 

```

这将在您的系统中安装 NetworkManager。现在我们必须启动网络管理服务。

```sh
      sudo systemctl start NetworkManager 

```

基于 Red Hat 的系统（如 Fedora、CentOS、REHL）：

```sh
      sudo dnf install NetworkManager 

```

这将在您的系统中安装 NetworkManager。现在我们必须启动网络管理服务。

```sh
      sudo systemctl start NetworkManager 

```

Arch Linux：

```sh
      sudo pacman -S networkmanager 

```

这将在您的系统中安装 NetworkManager。现在我们必须启动网络管理服务。

```sh
      sudo systemctl start NetworkManager 

```

### 示例：

1.  列出所有可用的 Wi-Fi 网络

```sh
      nmcli device wifi list 

```

1.  查看网络状态

```sh
      nmcli device status 

```

1.  连接到 Wi-Fi 网络

```sh
      nmcli device wifi connect "SSID_NAME" password "YOUR_PASSWORD" 

```

1.  从 Wi-Fi 网络断开连接

```sh
      nmcli connection down "CONNECTION_NAME" 

```

1.  打开/关闭 Wi-Fi

```sh
      nmcli radio wifi on
nmcli radio wifi off 

```

，分别。

1.  打开/关闭蓝牙

```sh
      nmcli radio bluetooth on
nmcli radio bluetooth off 

```

，分别。

1.  显示所有连接

```sh
      nmcli connection show 

```

1.  显示特定连接的详细信息

```sh
      nmcli connection show "CONNECTION_NAME" 

```

### 语法：

nmcli 命令的一般语法如下：

```sh
      nmcli [OPTIONS...] { help | general | networking | radio | connection | device | agent | monitor } [COMMAND] [ARGUMENTS...] 

```

### 其他标志及其功能：

#### 选项

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-a` | `--ask` | nmcli 将停止并请求任何缺少的必需参数（不要用于非交互式选项） |
| `-c` | `--color`{yes/no} | 它控制颜色输出。yes 启用颜色，而 no 禁用颜色 |
| `-h` | `--help` | 打印帮助信息 |
| `-p` | `--pretty` | 这将导致 nmcli 生成更用户友好的输出，例如带有标题，并且值对齐 |
| `-v` | `--version` | 显示 nmcli 版本 |
| `-f` | `--fields`{field1,...} | 此选项用于指定应打印哪些字段。有效字段名称因特定命令而异。 |
| `-g` | `--get-value`{field1,.} | 此选项用于打印特定字段的值。它是 --mode tabular --terse --fields 的快捷方式 |

#### 通用命令

| **命令** | **描述** |
| :-- | :-- |
| `nmcli general status` | 显示 NetworkManager 的总体状态 |
| `nmcli general hostname` | 显示当前主机名 |

#### 网络命令

| **命令** | **描述** |
| :-- | :-- |
| `nmcli networking on` | 启用所有网络 |
| `nmcli networking off` | 禁用所有网络 |
| `nmcli networking connectivity` | 检查网络连接状态 |

#### 无线电命令

| **命令** | **描述** |
| :-- | :-- |
| `nmcli radio wifi on` | 启用 Wi-Fi 无线电 |
| `nmcli radio wifi off` | 禁用 Wi-Fi 无线电 |
| `nmcli radio all` | 显示所有无线电开关的状态 |
| `nmcli radio wifi` | 显示 Wi-Fi 无线电状态 |

#### 连接管理命令

| **命令** | **描述** |
| :-- | :-- |
| `nmcli connection show` | 列出所有已保存的连接配置文件 |
| `nmcli connection show --active` | 仅列出活动连接 |
| `nmcli connection show "名称"` | 显示特定连接的详细信息 |
| `nmcli connection up "名称"` | 激活连接 |
| `nmcli connection down "名称"` | 使连接失效 |
| `nmcli connection modify "名称" [选项]` | 修改连接设置 |
| `nmcli connection delete "名称"` | 删除连接配置文件 |
| `nmcli connection reload` | 从磁盘重新加载所有连接文件 |

#### 设备管理命令

| **命令** | **描述** |
| :-- | :-- |
| `nmcli device status` | 显示所有设备的状态 |
| `nmcli device show "设备名称"` | 显示特定设备的详细信息 |
| `nmcli device disconnect "设备名称"` | 从设备断开连接 |
| `nmcli device wifi list` | 列出所有可用的 Wi-Fi 网络 |
| `nmcli device wifi connect "SSID" password "密码"` | 连接到受密码保护的 Wi-Fi |
