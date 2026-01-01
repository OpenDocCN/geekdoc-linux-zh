# `shutdown`命令

`shutdown`命令允许您以安全的方式关闭系统。当执行`shutdown`时，系统将通知所有已登录的用户并禁止进一步登录。您可以选择立即关闭系统或延迟特定时间后关闭。

只有具有 root（或 sudo）权限的用户才能使用`shutdown`命令。

### 示例：

1.  立即关闭您的系统：

```sh
      sudo shutdown now 

```

1.  在 10 分钟后关闭您的系统：

```sh
      sudo shutdown +10 

```

1.  在 5 分钟后关闭您的系统，并显示一条消息：

```sh
      sudo shutdown +5 "System will shutdown in 5 minutes" 

```

### 语法：

```sh
      shutdown [OPTIONS] [TIME] [MESSAGE] 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-r` | - | 重新启动系统 |
| `-c` | - | 取消已计划的关闭操作 |
