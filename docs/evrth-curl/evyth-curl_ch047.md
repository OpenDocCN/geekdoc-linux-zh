# Windows

您可以在 Windows 上以多种不同的方式构建 curl。我们推荐使用来自 Microsoft 的 MSVC 编译器或免费开源的 mingw 编译器。然而，构建过程并不仅限于这些。

如果您使用 mingw，您可能想使用 autotools 构建系统。

## winbuild

这就是使用命令行构建 curl 和 libcurl 的方法。

使用`nmake`工具以 MSVC 构建，如下所示：

```sh
cd winbuild
```

决定在您的构建中启用/禁用哪些选项。该目录中的`README.md`文件详细说明了所有选项，但一个示例命令行可能看起来像这样（为了可读性分成几行）：

```sh
nmake WITH_SSL=dll WITH_NGHTTP2=dll ENABLE_IPV6=yes \
WITH_ZLIB=dll MACHINE=x64 
```

## Visual C++项目文件

使用 CMake，您可以生成一组 Visual Studio 项目文件：

```sh
cmake -B build -G 'Visual Studio 17 2022'
```

一旦生成，您就可以导入它们，并像通常一样使用 Visual Studio 进行构建。

## Mingw

您可以使用 mingw 编译器套件构建 curl。使用 CMake 为您生成一组 Makefiles：

```sh
cmake -B build -G "MinGW Makefiles"
```
