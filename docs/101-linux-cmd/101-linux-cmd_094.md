# `pstree` 命令

`pstree` 命令类似于 `ps`，但它不是列出运行进程，而是以树形结构显示它们。树形格式有时是显示进程层次结构的更合适方式，这是一种更简单的方式来可视化运行进程。树的根是 init 或给定 pid 的进程。

### 示例

1.  要显示所有运行进程的分层树结构：

```sh
      pstree 

```

1.  要显示以给定进程为根的树形结构：

```sh
      pstree [pid] 

```

1.  要仅显示由用户启动的进程：

```sh
      pstree [USER] 

```

1.  要显示给定进程的父进程：

```sh
      pstree -s [PID] 

```

1.  要逐页查看输出，将其管道传输到 `less` 命令：

```sh
      pstree | less 

```

### 语法

`ps [选项] [用户或 PID]`

### 其他标志及其功能

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-a` | `--arguments` | 显示命令行参数 |
| `-A` | `--ascii` | 使用 ASCII 行绘图字符 |
| `-c` | `--compact` | 不压缩相同的子树 |
| `-h` | `--highlight-all` | 高亮显示当前进程及其祖先 |
| `-H PID` | `--highlight-pid=PID` | 高亮显示此进程及其祖先 |
| `-g` | `--show-pgids` | 显示进程组 ID；隐含 `-c` |
| `-G` | `--vt100` | 使用 VT100 行绘图字符 |
| `-l` | `--long` | 不截断长行 |
| `-n` | `--numeric-sort` | 按 PID 排序输出 |
| `-N type` | `--ns-sort=type` | 按命名空间类型（cgroup、ipc、mnt、net、pid、user、uts）排序 |
| `-p` | `--show-pids` | 显示 PIDs；隐含 -c |
| `-s` | `--show-parents` | 显示选定进程的父进程 |
| `-S` | `--ns-changes` | 显示命名空间转换 |
| `-t` | `--thread-names` | 显示完整的线程名称 |
| `-T` | `--hide-threads` | 隐藏线程，只显示进程 |
| `-u` | `--uid-changes` | 显示 uid 转换 |
| `-U` | `--unicode` | 使用 UTF-8（Unicode）行绘图字符 |
| `-V` | `--version` | 显示版本信息 |
| `-Z` | `--security-context` | 显示 SELinux 安全上下文 |
