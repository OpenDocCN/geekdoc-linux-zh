# `systemctl` 命令

`systemctl` 命令用于控制和管理工作系统中的 systemd 服务以及 systemd 系统和服务管理器。它是现代 Linux 发行版中管理服务的主要工具。

## 语法

```sh
      systemctl [options] command [service-name] 

```

## 常用命令

### 服务管理

```sh
      start [service]         Start a service
stop [service]          Stop a service
restart [service]       Restart a service
reload [service]        Reload service configuration
status [service]        Show service status
enable [service]        Enable service to start at boot
disable [service]       Disable service from starting at boot 

```

### 系统命令

```sh
      reboot                  Restart the system
poweroff               Shutdown the system
suspend                Suspend the system
hibernate              Hibernate the system 

```

## 选项

一些流行的选项标志包括：

```sh
      -l          Show full output (don't truncate)
--no-pager  Don't pipe output into a pager
--failed    Show only failed units
--all       Show all units, including inactive ones
-q          Quiet mode, suppress output
-t          Specify unit type (service, socket, etc.) 

```

## 示例

1.  启动一个服务

```sh
      systemctl start nginx 

```

1.  停止一个服务

```sh
      systemctl stop apache2 

```

1.  检查服务状态

```sh
      systemctl status ssh 

```

1.  启用服务以便在启动时启动

```sh
      systemctl enable mysql 

```

1.  禁用服务以便在启动时启动

```sh
      systemctl disable bluetooth 

```

1.  重新启动一个服务

```sh
      systemctl restart networking 

```

1.  不停止服务的情况下重新加载服务配置

```sh
      systemctl reload nginx 

```

1.  列出所有活跃的服务

```sh
      systemctl list-units --type=service 

```

1.  列出所有服务（活跃和不活跃）

```sh
      systemctl list-units --type=service --all 

```

1.  列出失败的服务

```sh
      systemctl --failed 

```

1.  显示服务依赖关系

```sh
      systemctl list-dependencies nginx 

```

1.  检查服务是否已启用

```sh
      systemctl is-enabled ssh 

```

1.  检查服务是否活跃

```sh
      systemctl is-active mysql 

```

1.  重新启动系统

```sh
      systemctl reboot 

```

1.  关闭系统

```sh
      systemctl poweroff 

```

## 服务状态信息

检查状态时，您将看到：

+   **活跃（运行）**：服务目前正在运行

+   **活跃（兴奋）**：服务完成成功

+   **不活跃（死亡）**：服务未运行

+   **失败**：服务启动失败

## 用例

+   管理网络服务器（nginx、apache）

+   控制数据库服务（mysql、postgresql）

+   管理系统服务（ssh、网络）

+   故障排除服务问题

+   在脚本中自动化服务管理

+   系统管理和维护

## 重要注意事项

+   大多数操作需要 root 权限（使用 `sudo`）

+   在 systemd 术语中，服务被称为“units”

+   配置文件位于 `/etc/systemd/system/`

+   在进行更改后始终检查服务状态

+   使用 `journalctl` 查看详细的服务日志

`systemctl` 命令对于现代 Linux 系统管理和服务管理至关重要。

更多详细信息，请查看手册：`man systemctl`
