# 单独安装

有时候，当你从源代码构建 curl 和 libcurl 时，你这样做是为了实验、测试或者可能是调试。在这些情况下，你可能还没有准备好替换你的系统级 libcurl 安装。

许多现代系统已经将 libcurl 安装在了系统中，所以当你构建和安装你的测试版本时，你需要确保你的新构建是为了你的目的而使用的。

我们收到了很多报告，说人们构建并安装了他们自己的 curl 和 libcurl 版本，但当他们随后调用他们新的 curl 构建时，新的工具找到了系统中的较旧的 libcurl，并使用它。这往往会让用户感到困惑。

## 静态链接

你可以通过与 libcurl 进行静态链接来避免 curl 找到较旧的动态 libcurl 库的问题。然而，这却引发了一系列其他挑战，因为将具有多个第三方依赖项的现代库静态链接是很困难的。当你静态链接时，你需要确保你向链接器提供了所有依赖项。这不是我们推荐的方法。

## 动态链接

当你在现代系统上调用`curl`时，有一个运行时链接器（通常称为`ld.so`），它会加载可执行文件构建时使用的共享库。共享库会在一组路径中搜索和加载。

问题通常在于系统 libcurl 库存在于那个路径中，而你的新构建的 libcurl 不存在。或者它们两个都存在于路径中，但系统的一个被首先找到。

在 Linux 系统上，运行时链接器的路径顺序通常定义在`/etc/ld.so.conf`中。你可以更改顺序，并且可以向搜索目录列表中添加新目录。记得在更新后运行`ldconfig`。

## 临时安装

如果你构建了一个 libcurl 并安装了它，你只想为单个应用程序使用它，或者只是想测试一下，编辑和更改动态库路径可能有点过于侵入性。

一个普通的 Unix 提供了一些其他我们推荐的替代方案。

### `LD_LIBRARY_PATH`

你可以在你的 shell 中设置这个环境变量，让运行时链接器在特定的目录中查找。这会影响在这个变量设置的所有可执行文件。

这对于快速检查来说很方便，或者如果你想要在不同的调用中使用不同的 libcurl，让你的单个`curl`可执行文件使用不同的 libcurl。

当你将你的新 curl 构建安装在`$HOME/install`时，它可能看起来像这样：

```sh
export LD_LIBRARY_PATH=$HOME/install/lib
$HOME/install/bin/curl https://example.com/
```

### `rpath`

通常，强制加载你自己的单独的 libcurl 而不是系统的一个更好的方法，是设置你构建的特定`curl`可执行文件的`rpath`。这给运行时链接器提供了一个特定的路径来检查这个特定的可执行文件。

这是在链接时完成的，如果你使用应用程序构建自己的 libcurl，你可以让它加载你的自定义 libcurl 构建，如下所示：

```sh
gcc -g example.c -L$HOME/install/lib -lcurl -Wl,-rpath=$HOME/install/lib
```

当设置了`rpath`后，链接到`$HOME/install/lib/libcurl.so`的可执行文件将使运行时链接器使用该特定路径和库，而系统中的其他二进制文件将继续使用系统库 curl。

当你想让你的自定义构建的`curl`使用它自己的 libcurl，并将它们安装到`$HOME/install`时，这个配置命令行可能看起来像这样：

```sh
LDFLAGS="-Wl,-rpath,$HOME/install/lib" ./configure ...
```

如果你的系统支持 rpath 的运行路径形式，通常最好使用它，因为它可以被`LD_LIBRARY_PATH`环境变量覆盖。它还可能在测试 curl 的树内构建时防止 libtool 错误，因为那时 libtool 可以使用`LD_LIBRARY_PATH`。较新的链接器在指定 rpath 时默认使用 rpath 的运行路径形式，但其他链接器可能需要一个额外的链接器标志`-Wl,--enable-new-dtags`，如下所示：

```sh
LDFLAGS="-Wl,-rpath,$HOME/install/lib -Wl,--enable-new-dtags" \
  ./configure ...
```
