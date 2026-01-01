# `yum`命令

`yum`命令是 Red Hat Enterprise Linux 中安装、更新、删除和管理软件包的主要包管理工具。它是*`Yellow Dog Updater, Modified`*的缩写。

`yum`在安装、更新和删除软件包时执行依赖关系解析。它可以管理系统中的已安装仓库或.rpm 软件包中的软件包。

### 语法：

```sh
      yum -option command 

```

### 示例：

1.  要查看过去事务中发生的情况概览：

```sh
      yum history 

```

1.  要撤销之前的交易：

```sh
      yum history undo <id> 

```

1.  要使用“是”作为所有确认的响应来安装 firefox 软件包

```sh
      yum -y install firefox 

```

1.  要将 mysql 软件包更新到最新稳定版本

```sh
      yum update mysql 

```

### 常用命令与 yum 一起使用：

| **命令** | **描述** |
| :-- | :-- |
| `install` | 安装指定的软件包 |
| `remove` | 删除指定的软件包 |
| `search` | 在软件包元数据中搜索关键字 |
| `info` | 列出描述 |
| `update` | 将每个软件包更新到最新版本 |
| `repolist` | 列出仓库 |
| `history` | 显示过去事务中发生的情况 |
| `groupinstall` | 安装特定的软件包组 |
| `clean` | 清除所有启用仓库中的缓存文件 |

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-C` | `--cacheonly` | 完全从系统缓存运行，不更新缓存，即使缓存已过期也使用它。 |
| - | `--security` | 包括提供安全问题的修复的软件包。适用于升级命令。 |
| `-y` | `--assumeyes` | 自动对所有问题回答“是”。 |
| - | `--skip-broken` | 通过从事务中删除导致问题的软件包来解决 depsolve 问题。它是具有 False 值的严格配置选项的别名。 |
| `-v` | `--verbose` | 详细操作，显示调试信息。 |
