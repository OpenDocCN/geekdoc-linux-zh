# 在 bash 或终端中，chmod o+w 命令是什么意思

> 原文：[`techbyexample.com/ow-chmod-permission-linux/`](https://techbyexample.com/ow-chmod-permission-linux/)

# **概览**

在管理文件权限时，有三个组成部分需要考虑。

+   **用户**—缩写为**‘u’**

+   **组**—缩写为**‘g’**

+   **其他**—缩写为**‘o’**

+   **权限**（读取/写入/执行），其中读取、写入、执行分别缩写为**‘r’、’w’**和**‘x’**

所以**o+w**意味着授予**其他**用户**写入**权限

# **示例**

+   创建文件**temp.txt**，检查其权限

```go
ls -all | grep temp.txt
-rw-r--r--    1 root  root      0 Aug  9 14:50 temp.txt
```

请注意，**其他**用户仅具有**读取**权限

+   现在运行命令

```go
chmod o+w temp.txt
ls -all | grep temp.txt
-rw-r--rw-    1 root  root      0 Aug  9 14:50 temp.txt
```

请查看上面的输出。执行权限也被授予给**其他**用户。
