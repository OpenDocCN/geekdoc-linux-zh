# `passwd` 命令

在 Linux 中，`passwd` 命令用于更改用户账户的密码。普通用户只能更改自己的账户密码，但超级用户可以更改任何账户的密码。`passwd` 命令还可以更改账户或相关密码的有效期。

## 示例

```sh
      $ passwd 

```

## `passwd` 命令的语法是：

```sh
      $ passwd [options] [LOGIN] 

```

## 选项

```sh
      -a, --all
        This option can be used only with -S and causes show status for all users.

-d, --delete
        Delete a user's password.

-e, --expire
        Immediately expire an account's password.

-h, --help
        Display help message and exit.

-i, --inactive
        This option is used to disable an account after the password has been expired for a number of days.

-k, --keep-tokens
        Indicate password change should be performed only for expired authentication tokens (passwords).

-l, --lock
        Lock the password of the named account.

-q, --quiet
        Quiet mode.

-r, --repository
        change password in repository.

-S, --status
        Display account status information. 

```
