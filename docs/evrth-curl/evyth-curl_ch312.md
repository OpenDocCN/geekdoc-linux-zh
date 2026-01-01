# Valgrind

Valgrind 是一个流行的强大工具，用于调试程序，尤其是它们对内存的使用和滥用。

`runtests.pl` 会自动检测你的系统上是否安装了 valgrind，并在默认情况下如果找到 valgrind 则使用它来运行测试。你可以传递 `-n` 参数给 runtests 以禁用 valgrind 的使用。

Valgrind 使得执行速度大大减慢，但它是一个用于查找内存泄漏和未初始化内存使用的优秀工具。
