# `chmod` 命令

`chmod` 命令允许您使用符号、数字或参考文件的方式更改文件的权限。

### 示例：

1.  使用符号模式更改文件的权限：

```sh
      chmod u=rwx,g=rx,o=r myfile 

```

上述命令表示：

+   用户可以读取、写入、执行 `myfile`

+   组可以读取、执行 `myfile`

+   其他用户可以读取 `myfile`

1.  使用数字模式更改文件的权限

```sh
      chmod 754 myfile user:group file.txt 

```

上述命令表示：

+   用户可以读取、写入、执行 `myfile`

+   组可以读取、执行 `myfile`

+   其他用户可以读取 `myfile`

1.  递归更改文件夹的权限

```sh
      chmod -R 754 folder 

```

### 语法：

```sh
      chmod [OPTIONS] MODE FILE(s) 

```

+   `[OPTIONS]` : `-R`: 递归，表示目录中的所有文件

+   `MODE`: 设置权限的不同方式：

+   **符号权限模式解释**：

    +   u: 用户

    +   g: 组

    +   o: 其它用户

    +   =: 设置权限

    +   r: 读取

    +   w: 写入

    +   x: 执行

    +   例如 `u=rwx` 表示用户可以读取、写入和执行

+   **数字权限模式解释**：

**数字权限模式**基于用户、组和其它权限的二进制表示，更多信息请参阅 Digital Ocean 社区部分提供的[解释](https://www.digitalocean.com/community/tutorials/linux-permissions-basics-and-how-to-use-umask-on-a-vps#types-of-permissions)：

+   4 代表 "读取",

+   2 代表 "写入",

+   1 代表 "执行"，并且

+   0 代表 "无权限。"

+   例如 7 表示读取 + 写入 + 执行
