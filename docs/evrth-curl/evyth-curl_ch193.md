# API 兼容性

libcurl 承诺 API 稳定性，并保证您今天编写的程序在未来仍然可以工作。我们不破坏兼容性。

随着时间的推移，我们在 API 中添加了新功能、新选项和新函数，但我们不会以不兼容的方式更改行为或删除函数。

我们上一次以不兼容的方式更改 API 是在 2006 年的 7.16.0 版本，我们计划永远不再这样做。

## 版本号

Curl 和 libcurl 分别进行版本控制，但它们通常非常接近。

版本编号始终使用相同的系统构建：

```sh
X.Y.Z
```

+   X 是主版本号

+   Y 是发布号

+   Z 是补丁号

## 增加数字

每个新的版本中，这些 X.Y.Z 数字中的一个都会增加。增加的数字右侧的数字都重置为 0。

当进行真正重大的、世界级的变化时，主版本号 X 会增加。当进行更改或添加功能时，发布号 Y 会增加。当更改仅仅是修复错误时，补丁号 Z 会增加。

这意味着在发布 1.2.3 之后，如果进行了真正重大的改进，我们可以发布 2.0.0，如果没有那么大的更改，我们可以发布 1.3.0，如果主要是修复了错误，我们可以发布 1.2.4。

增加，即通过加 1 来增加数字，无条件地只影响一个数字（及其右侧的数字设置为 0）。1 变为 2，3 变为 4，9 变为 10，88 变为 89，99 变为 100。因此，在 1.2.9 之后是 1.2.10。在 3.99.3 之后，可能是 3.100.0。

所有原始 curl 源发布存档都是根据 libcurl 版本命名的（而不是根据前面提到的可能不同的 curl 客户端版本）。 

## 哪个 libcurl 版本

作为一项服务，任何可能希望支持新的 libcurl 功能同时仍然能够使用旧版本构建的应用程序，所有发布版本都在 `curl/curlver.h` 文件中存储了 libcurl 版本，使用一个静态编号方案，可用于比较。版本号定义为：

```sh
#define LIBCURL_VERSION_NUM 0xXXYYZZ
```

其中 `XX`、`YY` 和 `ZZ` 分别是十六进制表示的主版本号、发布号和补丁号。所有三个数字字段始终使用两位数字（每个八位）表示。1.2.0 会显示为 `0x010200`，而版本 9.11.7 会显示为 `0x090b07`。

这个 6 位十六进制数在较新的版本中总是更大的数字。这使得使用大于和小于进行比较变得可行。

这个数字也作为三个单独的定义提供：`LIBCURL_VERSION_MAJOR`、`LIBCURL_VERSION_MINOR` 和 `LIBCURL_VERSION_PATCH`。

这些定义当然仅适用于确定刚刚构建的版本号，并不能帮助您确定三年后运行时使用的 libcurl 版本。

## 哪个 libcurl 版本在运行

为了确定您的应用程序当前正在使用哪个 libcurl 版本，`curl_version_info()` 函数可供您使用。

应用程序应使用此函数来判断是否可以进行某些操作，而不是使用编译时检查，因为动态/DLL 库可以在不依赖于应用程序的情况下进行更改。

`curl_version_info()` 返回一个指向包含版本号和 libcurl 运行版本中各种功能的 struct 的指针。您通过提供一个特殊的年龄计数器来调用它，这样 libcurl 就知道调用它的 libcurl 的年龄。年龄是一个名为 `CURLVERSION_NOW` 的宏，它是一个在整个 curl 开发过程中以不规则间隔增加的计数器。年龄数字告诉 libcurl 它可以返回哪个 struct 集合。

您可以这样调用该函数：

```sh
curl_version_info_data *version = curl_version_info( CURLVERSION_NOW );
```

数据随后指向一个具有以下布局的 struct：

```sh
struct {
  CURLversion age;          /* see description below */

  /* when 'age' is 0 or higher, the members below also exist: */
  const char *version;      /* human readable string */
  unsigned int version_num; /* numeric representation */
  const char *host;         /* human readable string */
  int features;             /* bitmask, see below */
  char *ssl_version;        /* human readable string */
  long ssl_version_num;     /* not used, always zero */
  const char *libz_version; /* human readable string */
  const char * const *protocols; /* protocols */

  /* when 'age' is 1 or higher, the members below also exist: */
  const char *ares;         /* human readable string */
  int ares_num;             /* number */

  /* when 'age' is 2 or higher, the member below also exists: */
  const char *libidn;       /* human readable string */

  /* when 'age' is 3 or higher (7.16.1 or later), the members below also
     exist  */
  int iconv_ver_num;       /* '_libiconv_version' if iconv enabled */

  const char *libssh_version; /* human readable string */

  /* when 'age' is 4 or higher, the member below also exists: */
  unsigned int brotli_ver_num; /* Numeric Brotli version
                                  (MAJOR << 24) | (MINOR << 12) | PATCH */
  const char *brotli_version; /* human readable string. */

  /* when 'age' is 5 or higher, the member below also exists: */
  unsigned int nghttp2_ver_num; /* Numeric nghttp2 version
                                   (MAJOR << 16) | (MINOR << 8) | PATCH */
  const char *nghttp2_version; /* human readable string. */
  const char *quic_version;    /* human readable quic (+ HTTP/3) library +
                                  version or NULL */

  /* when 'age' is 6 or higher, the member below also exists: */
  const char *cainfo;          /* built-in default CURLOPT_CAINFO, might
                                  be NULL */
  const char *capath;          /* built-in default CURLOPT_CAPATH, might
                                  be NULL */

  /* when 'age' is 7 or higher, the member below also exists: */
  unsigned int zstd_ver_num; /* Numeric Zstd version
                                  (MAJOR << 24) | (MINOR << 12) | PATCH */
  const char *zstd_version; /* human readable string. */

  /* when 'age' is 8 or higher, the member below also exists: */
  const char *hyper_version; /* human readable string. */

} curl_version_info_data;
```
