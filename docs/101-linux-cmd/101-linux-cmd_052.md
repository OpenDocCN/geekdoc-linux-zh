# `apt`命令

`apt`（优势软件包系统）命令用于与`dpkg`（由 debian 使用的软件包系统）交互。已经存在`dpkg`命令来管理`.deb`软件包。但`apt`是一个更用户友好且高效的途径。

简而言之，`apt`是一个用于在基于 debian 的 Linux 上安装、删除和执行其他操作的命令。

您将主要使用带有`sudo`权限的`apt`命令。

### 安装软件包：

使用`install`后跟`package_name`与`apt`一起安装新软件包。

##### 语法：

```sh
      sudo apt install package_name 

```

##### 示例：

```sh
      sudo apt install g++ 

```

此命令将在您的系统上安装 g++。

### 移除软件包：

使用`remove`后跟`package_name`与`apt`一起移除特定的软件包。

##### 语法：

```sh
      sudo apt remove package_name 

```

##### 示例：

```sh
      sudo apt remove g++ 

```

此命令将从您的系统中移除 g++。

### 搜索软件包：

使用`apt`搜索`package_name`，以在所有仓库中搜索软件包。

##### 语法：

```sh
      apt search package_name 

```

注意：无需 sudo 权限

##### 示例：

```sh
      apt search g++ 

```

### 移除未使用的软件包：

当在系统上安装依赖于其他软件包的新软件包时，也会安装这些软件包的依赖项。当软件包被移除时，依赖项将保留在系统中。这些遗留的软件包不再被其他任何东西使用，可以移除。

##### 语法：

```sh
      sudo apt autoremove 

```

此命令将从您的系统中移除所有未使用的软件包。

### 更新软件包索引：

`apt`软件包索引实际上是一个数据库，存储了在您的系统上启用的可用软件包的记录。

##### 语法：

```sh
      sudo apt update 

```

此命令将更新系统上的软件包索引。

### 升级软件包：

如果您想为已安装的软件包安装最新更新，您可能需要运行此命令。

##### 语法：

```sh
      sudo apt upgrade 

```

此命令不会升级需要移除已安装软件包的软件包。

如果您想升级单个软件包，请传递软件包名称：

##### 语法：

```sh
      sudo apt upgrade package_name 

```

此命令将升级您的软件包到最新版本。
