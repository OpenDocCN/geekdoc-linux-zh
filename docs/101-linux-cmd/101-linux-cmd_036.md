# `env` 命令

Linux/Unix 中的 `env` 命令用于打印当前环境变量列表，或者在不更改当前环境的情况下运行一个自定义环境中的程序。

## 语法

```sh
      env [OPTION]... [-] [NAME=VALUE]... [COMMAND [ARG]...] 

```

## 用法

1.  打印当前环境变量的集合

    ```sh
    env 
    ```

1.  使用空环境运行命令

    ```sh
     env -i command_name 
    ```

1.  从环境中删除变量

    ```sh
    env -u variable_name 
    ```

1.  每个输出以 NULL 结尾

    ```sh
    env -0 
    ```

## 选项完整列表

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-i` | `--ignore-environment` | 使用空环境启动 |
| `-0` | `--null` | 每个输出行以 NUL 结尾，而不是换行符 |
| `-u` | `--unset=NAME` | 从环境中删除变量 |
| `-C` | `--chdir=DIR` | 将工作目录更改为 DIR |
| `-S` | `--split-string=S` | 处理并分割 S 为单独的参数。它用于在 shebang 行上传递多个参数 |
| `-v` | `--debug` | 打印每个处理步骤的详细信息 |
| - | `--help` | 打印帮助信息 |
| - | `--version` | 打印版本信息 |
