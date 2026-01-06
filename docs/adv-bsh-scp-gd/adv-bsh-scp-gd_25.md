# 第二十章。I/O 重定向

> 原文：[`tldp.org/LDP/abs/html/io-redirection.html`](https://tldp.org/LDP/abs/html/io-redirection.html)

**目录**

20.1\. 使用 *exec*

20.2\. 重定向代码块

20.3\. 应用

总是有三个默认的 *文件* [[1]](#FTN.AEN17884) 是打开的，`stdin`（键盘）、`stdout`（屏幕）和 `stderr`（错误信息输出到屏幕）。这些，以及任何其他打开的文件，都可以进行重定向。重定向简单地说就是捕获来自文件、命令、程序、脚本或甚至脚本中的代码块（参见 示例 3-1 和 示例 3-2）的输出，并将其作为输入发送到另一个文件、命令、程序或脚本。

每个打开的文件都会分配一个文件描述符。 [[2]](#FTN.AEN17894) `stdin`、`stdout` 和 `stderr` 的文件描述符分别是 0、1 和 2。对于打开额外的文件，还剩下 3 到 9 的描述符。有时将其中一个额外的文件描述符分配给 `stdin`、`stdout` 或 `stderr` 作为临时的重复链接是有用的。 [[3]](#FTN.AEN17906) 这简化了在复杂重定向和重新排序后的恢复（参见 示例 20-1）。

```sh
   COMMAND_OUTPUT >
      # Redirect stdout to a file.
      # Creates the file if not present, otherwise overwrites it.

      ls -lR > dir-tree.list
      # Creates a file containing a listing of the directory tree.

   : > filename
      # The > truncates file "filename" to zero length.
      # If file not present, creates zero-length file (same effect as 'touch').
      # The : serves as a dummy placeholder, producing no output.

   > filename    
      # The > truncates file "filename" to zero length.
      # If file not present, creates zero-length file (same effect as 'touch').
      # (Same result as ": >", above, but this does not work with some shells.)

   COMMAND_OUTPUT >>
      # Redirect stdout to a file.
      # Creates the file if not present, otherwise appends to it.

      # Single-line redirection commands (affect only the line they are on):
      # --------------------------------------------------------------------

   1>filename
      # Redirect stdout to file "filename."
   1>>filename
      # Redirect and append stdout to file "filename."
   2>filename
      # Redirect stderr to file "filename."
   2>>filename
      # Redirect and append stderr to file "filename."
   &>filename
      # Redirect both stdout and stderr to file "filename."
      # This operator is now functional, as of Bash 4, final release.

   M>N
     # "M" is a file descriptor, which defaults to 1, if not explicitly set.
     # "N" is a filename.
     # File descriptor "M" is redirect to file "N."
   M>&N
     # "M" is a file descriptor, which defaults to 1, if not set.
     # "N" is another file descriptor.

      #==============================================================================

      # Redirecting stdout, one line at a time.
      LOGFILE=script.log

      echo "This statement is sent to the log file, \"$LOGFILE\"." 1>$LOGFILE
      echo "This statement is appended to \"$LOGFILE\"." 1>>$LOGFILE
      echo "This statement is also appended to \"$LOGFILE\"." 1>>$LOGFILE
      echo "This statement is echoed to stdout, and will not appear in \"$LOGFILE\"."
      # These redirection commands automatically "reset" after each line.

      # Redirecting stderr, one line at a time.
      ERRORFILE=script.errors

      bad_command1 2>$ERRORFILE       #  Error message sent to $ERRORFILE.
      bad_command2 2>>$ERRORFILE      #  Error message appended to $ERRORFILE.
      bad_command3                    #  Error message echoed to stderr,
                                      #+ and does not appear in $ERRORFILE.
      # These redirection commands also automatically "reset" after each line.
      #=======================================================================
```
```sh
   2>&1
      # Redirects stderr to stdout.
      # Error messages get sent to same place as standard output.
        >>filename 2>&1
            bad_command >>filename 2>&1
            # Appends both stdout and stderr to the file "filename" ...
        2>&1 &#124; [command(s)]
            bad_command 2>&1 &#124; awk '{print $5}'   # found
            # Sends stderr through a pipe.
            # &#124;& was added to Bash 4 as an abbreviation for 2>&1 &#124;.

   i>&j
      # Redirects file descriptor *i* to *j*.
      # All output of file pointed to by *i* gets sent to file pointed to by *j*.

   >&j
      # Redirects, by default, file descriptor *1* (stdout) to *j*.
      # All stdout gets sent to file pointed to by *j*.
```  |
```sh
   0< FILENAME
    < FILENAME
      # Accept input from a file.
      # Companion command to ">", and often used in combination with it.
      #
      # grep search-word <filename

   [j]<>filename
      #  Open file "filename" for reading and writing,
      #+ and assign file descriptor "j" to it.
      #  If "filename" does not exist, create it.
      #  If file descriptor "j" is not specified, default to fd 0, stdin.
      #
      #  An application of this is writing at a specified place in a file. 
      echo 1234567890 > File    # Write string to "File".
      exec 3<> File             # Open "File" and assign fd 3 to it.
      read -n 4 <&3             # Read only 4 characters.
      echo -n . >&3             # Write a decimal point there.
      exec 3>&-                 # Close fd 3.
      cat File                  # ==> 1234.67890
      #  Random access, by golly.

   &#124;
      # Pipe.
      # General purpose process and command chaining tool.
      # Similar to ">", but more general in effect.
      # Useful for chaining commands, scripts, files, and programs together.
      cat *.txt &#124; sort &#124; uniq > result-file
      # Sorts the output of all the .txt files and deletes duplicate lines,
      # finally saves results to "result-file".
```  |

可以在单个命令行中组合多个输入和输出重定向和/或管道实例。

```sh
command < input-file > output-file
# Or the equivalent:
< input-file command > output-file   # Although this is non-standard.

command1 &#124; command2 &#124; command3 > output-file
```

请参阅 示例 16-31 和 示例 A-14。

可以将多个输出流重定向到一个文件。

```sh
ls -yz >> command.log 2>&1
#  Capture result of illegal options "yz" in file "command.log."
#  Because stderr is redirected to the file,
#+ any error messages will also be there.

#  Note, however, that the following does *not* give the same result.
ls -yz 2>&1 >> command.log
#  Outputs an error message, but does not write to file.
#  More precisely, the command output (in this case, null)
#+ writes to the file, but the error message goes only to stdout.

#  If redirecting both stdout and stderr,
#+ the order of the commands makes a difference.
```

**关闭文件描述符**

n<&-

关闭输入文件描述符 `*n*`。

0<&-, <&-

关闭 `stdin`。

n>&-

关闭输出文件描述符 `*n*`。

1>&-, >&-

关闭 `stdout`。

子进程会继承打开的文件描述符。这就是管道为什么能工作的原因。为了防止 fd 被继承，请关闭它。

```sh
# Redirecting only stderr to a pipe.

exec 3>&1                              # Save current "value" of stdout.
ls -l 2>&1 >&3 3>&- &#124; grep bad 3>&-    # Close fd 3 for 'grep' (but not 'ls').
#              ^^^^   ^^^^
exec 3>&-                              # Now close it for the remainder of the script.

# Thanks, S.C.
```

有关 I/O 重定向的更详细介绍，请参阅 附录 F。

### 注意事项

| [[1]](io-redirection.html#AEN17884) | 在 UNIX 和 Linux 中，按照惯例，数据流和外设（设备文件）被视为文件，类似于普通文件的处理方式。 |
| --- | --- |
| [[2]](io-redirection.html#AEN17894) | 文件描述符只是一个操作系统分配给打开文件的数字，用于跟踪它。将其视为一种简化的文件指针。它与 **C** 中的 *文件句柄* 类似。 |
| [[3]](io-redirection.html#AEN17906) | 使用 `*文件描述符 5*` 可能会引起问题。当 Bash 创建子进程时，就像 exec 一样，子进程会继承 fd 5（参见 Chet Ramey 的存档电子邮件，[主题：RE: 文件描述符 5 保持打开](http://groups.google.com/group/gnu.bash.bug/browse_thread/thread/13955daafded3b5c/18c17050087f9f37)）。最好让这个特定的 fd 保持不动。 |
