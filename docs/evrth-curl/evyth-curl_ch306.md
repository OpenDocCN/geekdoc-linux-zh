# 运行测试

运行测试的主要脚本被称为 `tests/runtests.pl`，其中一些更有用的特性包括：

## 运行一系列测试

运行测试 1 到 27：

```sh
./runtests.pl 1 to 27
```

运行所有标记为 `SFTP` 的测试：

```sh
./runtests.pl SFTP
```

运行所有未标记为 `FTP` 的测试：

```sh
./runtests.pl '!FTP'
```

## 使用 gdb 运行特定测试

```sh
./runtests.pl -g 144
```

它会启动 gdb，你可以设置断点等，然后输入 `run` 并开始运行，整个测试过程都会通过调试器执行。

## 不使用 valgrind 运行特定测试

如果测试套件找到了 valgrind，它默认会使用它，这是一个寻找问题的极好方法，但同时也使得测试运行速度大大减慢。有时你希望它运行得更快：

```sh
./runtests.pl -n 144
```
