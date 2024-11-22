# 如何在你的机器上找到 /bin/sh 指向的内容

> 原文：[`techbyexample.com/bin-sh-points/`](https://techbyexample.com/bin-sh-points/)

# **概述**

**/bin/sh** 可以是符号链接或硬链接，指向它所指向的目标。/bin/sh 可以指向

+   /bin/bash

+   /bin/dash

+   ….

# **如果 /bin/sh 是符号链接**

如果 /bin/sh 是符号链接，我们可以使用以下命令查找它指向的目标。

```go
file -h /bin/sh
```

如果 **/bin/sh** 是指向 bash 的符号链接，那么输出将是：

```go
/bin/sh: symbolic link to bash
```

# **如果 /bin/sh 是硬链接**

在这种情况下，我们可以使用以下命令查找 /bin/sh 指向的目标。

```go
find -L /bin -samefile /bin/sh
```

如果它指向 dash，那么输出将如下：

```go
/bin/dash
/bin/sh
```

例如，在 Docker 的 Ubuntu 容器中，**/bin/sh** 指向 **/bin/dash**。所以，如果你在 Docker 的 Ubuntu 容器中运行上述命令，它将输出 /bin/dash。

要在 Docker 容器中运行的命令

```go
docker run -it ubuntu  find -L /bin -samefile /bin/sh
```

**输出**

```go
/bin/dash
/bin/sh
```

**注意：** 请查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
