# `userdel` 命令

`userdel` 命令用于删除用户账户及其相关文件

### 示例：

要使用 `userdel` 命令删除用户，语法如下：

```sh
      userdel userName 

```

要强制删除用户账户，即使用户仍然登录，使用 `userdel` 命令的语法如下：

```sh
      userdel -f userName 

```

要使用 `userdel` 命令删除用户及其主目录中的文件，语法如下：

```sh
      userdel -r userName 

```

### 语法：

```sh
      userdel [OPTIONS] userName 

```

### 可能的选项：

| **标志** | **描述** |
| :-- | :-- |
| `-f` | 强制删除指定的用户账户，即使用户已登录 |
| `-r` | 删除用户主目录中的文件以及主目录本身和用户的邮件队列 |
| `-Z` | 删除用户登录的任何 SELinux（安全增强型 Linux）用户映射。 |
