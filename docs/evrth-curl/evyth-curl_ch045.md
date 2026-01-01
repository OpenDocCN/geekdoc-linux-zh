# CMake

CMake 是一种适用于大多数现代平台（包括 Windows）的替代构建方法。使用这种方法，你首先需要在构建机器上安装 cmake，调用 cmake 生成构建文件，然后进行构建。使用 cmake 的`-G`标志，你可以选择为哪个构建系统生成文件。查看`cmake --help`以获取你的 cmake 安装支持的“生成器”列表。

在 cmake 命令行上，第一个参数指定了查找 cmake 源文件的位置，如果是在同一目录下，则为`.`（一个点）。

在 Linux 上使用纯 make 和同一目录下的 CMakeLists.txt 文件进行构建，你可以这样做：

```sh
cmake -G "Unix Makefiles" .
make
```

或者依赖这样一个事实：在 Unix 系统中，makefiles 是默认的：

```sh
cmake .
make
```

要创建一个用于构建的子目录并在其中运行 make，可以这样做：

```sh
mkdir build
cd build
cmake ..
make
```
