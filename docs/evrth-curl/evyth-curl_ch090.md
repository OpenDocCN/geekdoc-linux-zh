# Shell 重定向

当你在 shell 或某些其他命令行提示系统中调用 curl 时，该环境通常会为你提供一组输出重定向能力。在大多数 Linux 和 Unix shell 以及 Windows 的命令提示符中，你可以使用`>`将标准输出重定向到文件。当然，使用这种方法，-o 或-O 选项就变得多余了。

```sh
curl http://example.com/ > example.html
```

将输出重定向到文件会将 curl 的所有输出重定向到该文件，因此即使你要求将多个 URL 的输出传输到 stdout，重定向输出也会将所有 URL 的输出存储在该单个文件中。

```sh
curl http://example.com/1 http://example.com/2 > files
```

Unix shell 通常允许你单独重定向*stderr*流。stderr 流通常是一个也会在终端中显示的流，但你可以在不与 stdout 流混淆的情况下单独重定向它。stdout 流用于数据，而 stderr 用于元数据和错误等非数据。你可以像这样使用`2>file`来重定向 stderr：

```sh
curl http://example.com > files.html 2>errors
```
