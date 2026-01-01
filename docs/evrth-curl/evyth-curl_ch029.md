# MSYS2

[MSYS2](https://www.msys2.org/)是基于[mingw-w64](https://www.mingw-w64.org/)的流行 Windows 构建系统，包括 gcc 和 clang 编译器。MSYS2 使用名为`pacman`（从 arch-linux 移植而来）的包管理器，并拥有大约 2000 个预编译的[mingw-packages](https://github.com/msys2/MINGW-packages)。MSYS2 旨在构建独立软件：使用 mingw-w64 编译器构建的二进制文件不依赖于 MSYS2 本身[¹]。

## 在 MSYS2 上获取 curl 和 libcurl

关于[`mingw-w64-curl`](https://github.com/msys2/MINGW-packages/blob/master/mingw-w64-curl/PKGBUILD)包的当前信息可以在 msys2 网站上找到：https://packages.msys2.org/base/mingw-w64-curl。我们还可以在此找到各种可用版本的安装说明。例如，要安装 curl 的默认 x64 二进制文件，我们运行：

```sh
pacman -Sy mingw-w64-x86_64-curl
```

此包包含`curl`命令行工具以及 libcurl 头文件和共享库。默认的`curl`包使用 OpenSSL 后端构建，因此依赖于`mingw-w64-x86_64-openssl`。还有`mingw-w64-x86_64-curl-gnutls`和`mingw-w64-x86_64-curl-gnutls`包，有关更多详细信息，请参阅[msys2 网站](https://packages.msys2.org/base/mingw-w64-curl)。

就像在 Linux 上一样，我们可以使用`pkg-config`来查询构建针对 libcurl 所需的标志。使用 mingw64 shell（它会自动设置路径以包含`/mingw64`）启动 msys2 并运行：

```sh
pkg-config --cflags libcurl
# -IC:/msys64/mingw64/include

pkg-config --libs libcurl
# -LC:/msys64/mingw64/lib -lcurl
```

pacman 包管理器安装预编译的二进制文件。接下来，我们将解释如何使用 pacman 本地构建 curl，例如自定义配置。

## 在 MSYS2 上构建 libcurl

使用 pacman 构建包几乎和安装一样简单。整个流程都包含在`mingw-w64-curl`包的[PKGBUILD](https://github.com/msys2/MINGW-packages/blob/master/mingw-w64-curl/PKGBUILD)文件中。我们可以轻松修改该文件以自行重新构建包。

如果我们从干净的 msys2 安装开始，我们首先想要安装一些构建工具，如`autoconf`、`patch`和`git`。启动 msys2 shell 并运行：

```sh
# Sync the repositories
pacman -Syu

# Install git, autoconf, patch, etc
pacman -S git base-devel

# Install GCC for x86_64
pacman -S mingw-w64-x86_64-toolchain
```

现在克隆`mingw-packages`仓库并转到`mingw-w64-curl`包：

```sh
git clone https://github.com/msys2/MINGW-packages
cd MINGW-packages/mingw-w64-curl
```

此目录包含用于构建 curl 的 PKGBUILD 文件和补丁。查看 PKGBUILD 文件以了解发生了什么。现在要编译它，我们可以这样做：

```sh
makepkg-mingw --syncdeps --skippgpcheck
```

就这样。`--syncdeps`参数会自动检查并提示安装`mingw-w64-curl`的依赖项（如果尚未安装）。一旦过程完成，当前目录中就会有 3 个新文件，例如：

+   `pacman -U mingw-w64-x86_64-curl-7.80.0-1-any.pkg.tar.zst`

+   `pacman -U mingw-w64-x86_64-curl-gnutls-7.80.0-1-any.pkg.tar.zst`

+   `pacman -U mingw-w64-x86_64-curl-winssl-7.80.0-1-any.pkg.tar.zst`

使用`pacman -u`命令安装此类本地包文件：

```sh
pacman -U mingw-w64-x86_64-curl-winssl-7.80.0-1-any.pkg.tar.zst
```

查看 [msys2 文档](https://www.msys2.org/docs/package-management/) 或加入 [gitter](https://gitter.im/msys2/msys2) 以了解更多关于使用 pacman 和 msys2 进行构建的信息。

[¹]: 请注意不要将 [mingw-package](https://github.com/msys2/MINGW-packages) 的 `mingw-w64-curl` 与 [msys-packages](https://github.com/msys2/MSYS2-packages) 的 `curl` 和 `curl-devel` 混淆。后者是 msys2 环境本身的一部分（例如，用于支持 pacman 下载），但不适合分发。要构建不依赖于 MSYS2 本身的可分发软件，你始终需要 `mingw-w64-…` 软件包和工具链。
