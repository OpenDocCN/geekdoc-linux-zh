# 如何查找附加到服务器的磁盘和卷的大小/空间

> 原文：[`techbyexample.com/find-the-size-of-disks-and-volumes-attached-to-the-server/`](https://techbyexample.com/find-the-size-of-disks-and-volumes-attached-to-the-server/)

## 使用‘df -h’命令

**语法**

```go
df -h Mount name/Filesystem
```

**示例：**

```go
[root@macbpro git-spec]# df -h
filesystem      Size  Used Avail Use% Mounted on
devtmpfs        7.8G     0  7.8G   0% /dev
tmpfs           7.8G  332K  7.8G   1% /dev/shm
tmpfs           7.8G   49M  7.8G   1% /run
tmpfs           7.8G     0  7.8G   0% /sys/fs/cgroup
/dev/vda1        99G   53G   42G  57% /
tmpfs           512M  174M  339M  34% /tmp
tmpfs           1.6G     0  1.6G   0% /run/user/0

[root@macbpro git-spec]# 

[root@macbpro git-spec]# df -h /
filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        99G   53G   42G  57% /

[root@macbpro git-spec]# df -h /dev
filesystem      Size  Used Avail Use% Mounted on
devtmpfs        7.8G     0  7.8G   0% /dev

[root@macbpro git-spec]# 

[root@macbpro git-spec]# df -h /dev/vda1
filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        99G   53G   42G  57% /

[root@macbpro git-spec]# 
```

**注意：** 对于“文件系统”和“挂载名称”，请分别参考第一个示例中的第一列和最后一列。
