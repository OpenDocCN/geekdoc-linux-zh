# `hostnamectl` 命令

`hostnamectl` 命令提供了一个用于控制 Linux 系统主机名及其相关设置的适当 API。该命令还帮助在不实际定位和编辑给定系统上的 `/etc/hostname` 文件的情况下更改主机名。

## 语法

```sh
      $ hostnamectl [OPTIONS...] COMMAND ... 

```

其中 **COMMAND** 可以是以下任何一项

**status**：用于检查当前主机名设置

**set-hostname NAME**：用于设置系统主机名

**set-icon-name NAME**：用于为主机设置图标名称

## 示例

1.  查看当前主机名的基本用法

```sh
      $ hostnamectl 

```

或者

```sh
      $ hostnamectl status 

```

1.  将静态主机名更改为 *myhostname*。这可能需要或不需要 root 权限

```sh
      $ hostnamectl set-hostname myhostname --static 

```

1.  设置或更改临时主机名

```sh
      $ hostnamectl set-hostname myotherhostname --transient 

```

1.  设置美观的主机名。要设置的名字需要放在双引号（” “）内。

```sh
      $ hostname set-hostname "prettyname" --pretty 

```
