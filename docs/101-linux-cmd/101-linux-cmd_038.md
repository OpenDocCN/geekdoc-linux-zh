# `hostname` 命令

`hostname` 用于显示系统的 DNS 名称，以及显示或设置其主机名或 NIS 域名。

### 语法：

```sh
      hostname [-a|--alias] [-d|--domain] [-f|--fqdn|--long] [-A|--all-fqdns] [-i|--ip-address] [-I|--all-ip-addresses] [-s|--short] [-y|--yp|--nis] 

```

### 示例：

1.  `hostname -a, hostname --alias` 显示主机别名（如果已使用）。此选项已弃用，不应再使用。

1.  `hostname -s, hostname --short` 显示短主机名。这是在第一个点处截断的主机名。

1.  `hostname -V, hostname --version` 在标准输出上打印版本信息并成功退出。

### 帮助命令

运行以下命令以查看`hostname`命令的完整指南。

```sh
      man hostname 

```
