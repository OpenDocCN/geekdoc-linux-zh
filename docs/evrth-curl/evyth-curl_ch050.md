# BoringSSL

## 构建 boringssl

`$HOME/src` 是我在本例中放置代码的位置。您可以选择任何您喜欢的位置。

```sh
$ cd $HOME/src
$ git clone https://boringssl.googlesource.com/boringssl
$ cd boringssl
$ mkdir build
$ cd build
$ cmake -DCMAKE_POSITION_INDEPENDENT_CODE=on ..
$ make
```

## 设置构建树以便 curl 的配置程序能够检测到

在 boringssl 源代码树根目录下，确保存在一个 `lib` 目录和一个 `include` 目录。`lib` 目录应包含两个库（我将它们作为符号链接放入构建目录）。`include` 目录默认已经存在。按照以下方式构建和填充 `lib`（在源代码树根目录下执行命令，而不是在 `build/` 子目录下）。

```sh
$ mkdir lib
$ cd lib
$ ln -s ../build/ssl/libssl.a
$ ln -s ../build/crypto/libcrypto.a
```

## 配置 curl

`LIBS=-lpthread ./configure --with-ssl=$HOME/src/boringssl`（在此我指出 boringssl 树的根目录）

在配置结束时，确认它显示检测到使用了 BoringSSL。

## 构建 curl

在 curl 源代码树中运行 `make`。

现在，您可以使用 `make install` 等命令正常安装 curl。
