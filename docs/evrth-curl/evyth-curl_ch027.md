# Linux

Linux 发行版自带了包管理器，允许您安装它们提供的软件。大多数 Linux 发行版如果默认没有安装，会提供 curl 和 libcurl 的安装选项。

## Ubuntu 和 Debian

`apt`是用于在 Debian Linux 和 Ubuntu Linux 及其衍生版上安装预构建包的工具。

为了安装 curl 命令行工具，通常您只需

```sh
apt install curl
```

确保依赖项已安装，通常 libcurl 也会作为一个单独的包一起安装。

如果您想针对 libcurl 构建应用程序，您需要安装一个开发包以获取包含头文件和一些额外文档等。然后您可以选择您喜欢的具有 TLS 后端的 libcurl：

```sh
apt install libcurl4-openssl-dev
```

或者

```sh
apt install libcurl4-gnutls-dev
```

## Redhat 和 CentOS

在 Redhat Linux 和 CentOS Linux 衍生版中，您使用`yum`来安装包。使用以下命令安装命令行工具：

```sh
yum install curl
```

您可以使用以下方式安装 libcurl 开发包（包括包含文件和一些文档等）：

```sh
yum install libcurl-devel
```

## Fedora

Fedora Workstation 和其他基于 Fedora 的发行版使用`dnf`来安装包。

使用以下命令安装命令行工具：

```sh
dnf install curl
```

为了安装 libcurl 开发包，您需要运行：

```sh
dnf install libcurl-devel
```

## 不可变的 Fedora 发行版

如 Silverblue、Kinoite、Sericea、Onyx 等发行版使用`rpm-ostree`来安装包。请记住在安装后重启系统。

```sh
rpm-ostree install curl
```

为了安装 libcurl 开发包，您需要运行：

```sh
rpm-ostree install libcurl-devel
```

## nix

[Nix](https://nixos.org/nix/)是 NixOS 发行版的默认包管理器，但它也可以在任何 Linux 发行版中使用。

为了安装命令行工具：

```sh
nix-env -i curl
```

## Arch Linux

curl 位于 Arch Linux 的核心仓库中。这意味着如果您遵循正常的安装程序，它应该会自动安装。

如果 curl 未安装，Arch Linux 使用`pacman`来安装包：

```sh
pacman -S curl
```

## SUSE 和 openSUSE

在 SUSE Linux 和 openSUSE Linux 中，您使用`zypper`来安装包。为了安装 curl 命令行工具：

```sh
zypper install curl
```

为了安装 libcurl 开发包，您需要运行：

```sh
zypper install libcurl-devel
```

### SUSE SLE Micro 和 openSUSE MicroOS

这些版本的 SUSE/openSUSE Linux 是不可变的操作系统，具有只读的根文件系统，为了安装包，您将使用`transactional-update`而不是`zypper`。为了安装 curl 命令行工具：

```sh
transactional-update pkg install curl
```

并且为了安装 libcurl 开发包：

```sh
transactional-update pkg install libcurl-devel
```

## Gentoo

此包安装了工具、libcurl、头文件和 pkg-config 文件等

```sh
emerge net-misc/curl
```

## Void Linux

在 Void Linux 中，您使用`xbps-install`来安装包。为了安装 curl 命令行工具：

```sh
xbps-install curl
```

为了安装 libcurl 开发包：

```sh
xbps-install libcurl-devel
```
