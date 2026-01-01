# `useradd`命令

`useradd`命令用于向系统添加或更新用户账户。

### 示例：

使用`useradd`命令添加新用户的语法如下：

```sh
      useradd NewUser 

```

使用`useradd`命令添加新用户并为该新用户指定家目录路径的语法如下：

```sh
      useradd -d /home/NewUser NewUser 

```

使用`useradd`命令添加新用户并为其指定特定 ID 的语法如下：

```sh
      useradd -u 1234 NewUser 

```

### 语法：

```sh
      useradd [OPTIONS] NameOfUser 

```

### 可能的选项：

| **Flag** | **描述** | **参数** |
| :-- | :-- | :-- |
| `-d` | 新用户将使用/path/to/directory 作为用户登录目录的值创建 | /path/to/directory |
| `-u` | 用户 ID 的数值 | ID |
| `-g` | 使用特定组 ID 创建用户 | GroupID |
| `-M` | 创建不带家目录的用户 | - |
| `-e` | 使用到期日期创建用户 | DATE (格式：YYYY-MM-DD) |
| `-c` | 使用注释创建用户 | COMMENT |
| `-s` | 使用更改后的登录 shell 创建用户 | /path/to/shell |
| `-p` | 为用户设置未加密的密码 | PASSWORD |
