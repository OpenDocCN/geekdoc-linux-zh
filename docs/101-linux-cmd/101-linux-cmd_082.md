# `usermod` 命令

`usermod` 命令允许您通过命令行更改 Linux 中用户的属性。在创建用户后，我们有时需要更改他们的属性，如密码或登录目录等。因此，为了做到这一点，我们使用 `usermod` 命令。

### 语法：

```sh
      usermod [options] USER 

```

#### 注意：只有超级用户（root）可以执行 `usermod` 命令

### 选项及其功能：

| **选项** | **描述** |
| :-- | :-- |
| `-a` | 将组中的任何人添加到次要组 |
| `-c` | 为用户账户添加注释字段 |
| `-d` | 修改任何现有用户账户的目录 |
| `-g` | 更改用户的默认组 |
| `-G` | 添加补充组 |
| `-l` | 更改现有的用户登录名 |
| `-L` | 锁定系统用户账户 |
| `-m` | 将现有主目录的内容移动到新目录 |
| `-p` | 创建一个未加密的密码 |
| `-s` | 为新账户创建指定的 shell |
| `-u` | 为用户账户分配 UID |
| `-U` | 解锁任何被锁定的用户 |

### 示例：

1.  要为用户添加注释/描述：

```sh
      sudo usermod -c "This is test user" test_user 

```

1.  要更改用户的家目录：

```sh
      sudo usermod -d /home/sam test_user 

```

1.  要更改用户的过期日期：

```sh
      sudo usermod -e 2021-10-05 test_user 

```

1.  要更改用户的组：

```sh
      sudo usermod -g sam test_user 

```

1.  要更改用户登录名：

```sh
      sudo usermod -l test_account test_user 

```

1.  要锁定用户：

```sh
      sudo usermod -L test_user 

```

1.  要解锁用户：

```sh
      sudo usermod -U test_user 

```

1.  要为用户设置未加密的密码：

```sh
      sudo usermod -p test_password test_user 

```

1.  要为用户创建一个 shell：

```sh
      sudo usermod -s /bin/sh test_user 

```

1.  要更改用户的用户 ID：

```sh
      sudo usermod -u 1234 test_user 

```
