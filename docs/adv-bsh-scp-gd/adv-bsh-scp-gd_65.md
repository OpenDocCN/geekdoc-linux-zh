# 附录 K. 本地化

> 原文：[`tldp.org/LDP/abs/html/localization.html`](https://tldp.org/LDP/abs/html/localization.html)

本地化是 Bash 的一个未记录功能。

本地化的 shell 脚本会以系统区域设置定义的语言输出其文本输出。德国柏林的 Linux 用户将获得德语的脚本输出，而他在马里兰州柏林的堂兄将获得相同脚本的英文输出。

要创建本地化脚本，请使用以下模板将所有消息写入用户（错误消息、提示等）。

```sh
#!/bin/bash
# localized.sh
#  Script by Stphane Chazelas,
#+ modified by Bruno Haible, bugfixed by Alfredo Pironti.

. gettext.sh

E_CDERROR=65

error()
{
  printf "$@" >&2
  exit $E_CDERROR
}

cd $var &#124;&#124; error "`eval_gettext \"Can\'t cd to \\\$var.\"`"
#  The triple backslashes (escapes) in front of $var needed
#+ "because eval_gettext expects a string
#+ where the variable values have not yet been substituted."
#    -- per Bruno Haible
read -p "`gettext \"Enter the value: \"`" var
#  ...

#  ------------------------------------------------------------------
#  Alfredo Pironti comments:

#  This script has been modified to not use the $"..." syntax in
#+ favor of the "`gettext \"...\"`" syntax.
#  This is ok, but with the new localized.sh program, the commands
#+ "bash -D filename" and "bash --dump-po-string filename"
#+ will produce no output
#+ (because those command are only searching for the $"..." strings)!
#  The ONLY way to extract strings from the new file is to use the
# 'xgettext' program. However, the xgettext program is buggy.

# Note that 'xgettext' has another bug.
#
# The shell fragment:
#    gettext -s "I like Bash"
# will be correctly extracted, but . . .
#    xgettext -s "I like Bash"
# . . . fails!
#  'xgettext' will extract "-s" because
#+ the command only extracts the
#+ very first argument after the 'gettext' word.

#  Escape characters:
#
#  To localize a sentence like
#     echo -e "Hello\tworld!"
#+ you must use
#     echo -e "`gettext \"Hello\\tworld\"`"
#  The "double escape character" before the `t' is needed because
#+ 'gettext' will search for a string like: 'Hello\tworld'
#  This is because gettext will read one literal `\')
#+ and will output a string like "Bonjour\tmonde",
#+ so the 'echo' command will display the message correctly.
#
#  You may not use
#     echo "`gettext -e \"Hello\tworld\"`"
#+ due to the xgettext bug explained above.

# Let's localize the following shell fragment:
#     echo "-h display help and exit"
#
# First, one could do this:
#     echo "`gettext \"-h display help and exit\"`"
#  This way 'xgettext' will work ok,
#+ but the 'gettext' program will read "-h" as an option!
#
# One solution could be
#     echo "`gettext -- \"-h display help and exit\"`"
#  This way 'gettext' will work,
#+ but 'xgettext' will extract "--", as referred to above.
#
# The workaround you may use to get this string localized is
#     echo -e "`gettext \"\\0-h display help and exit\"`"
#  We have added a \0 (NULL) at the beginning of the sentence.
#  This way 'gettext' works correctly, as does 'xgettext.'
#  Moreover, the NULL character won't change the behavior
#+ of the 'echo' command.
#  ------------------------------------------------------------------
```
```sh
bash$ **bash -D localized.sh**
"Can't cd to %s."
 "Enter the value: "
```  |

这列出了所有本地化文本。（`-D` 选项列出了以 $ 为前缀的双引号字符串，而不执行脚本。）

```sh
bash$ **bash --dump-po-strings localized.sh**
#: a:6
 msgid "Can't cd to %s."
 msgstr ""
 #: a:7
 msgid "Enter the value: "
 msgstr ""
```

Bash 的 `--dump-po-strings` 选项类似于 `-D` 选项，但使用 gettext 的 "po" 格式。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | Bruno Haible 指出：从 gettext-0.12.2 版本开始，建议使用 **xgettext -o - localized.sh** 而不是 **bash --dump-po-strings localized.sh**，因为 **xgettext** . . .1\. 理解 gettext 和 eval_gettext 命令（而 bash --dump-po-strings 只理解其已弃用的 $"..." 语法）2\. 可以提取程序员放置的注释，这些注释旨在由翻译者阅读。这个 shell 代码现在不再特定于 Bash；它以相同的方式与 Bash 1.x 和其他 /bin/sh 实现一起工作。 |
| --- | --- |

现在，为脚本将要翻译成每种语言创建一个 `language.po` 文件，并指定 `*msgstr*`。Alfredo Pironti 提供了以下示例：

fr.po:

```sh
#: a:6
msgid "Can't cd to $var."
msgstr "Impossible de se positionner dans le repertoire $var."
#: a:7
msgid "Enter the value: "
msgstr "Entrez la valeur : "

#  The string are dumped with the variable names, not with the %s syntax,
#+ similar to C programs.
#+ This is a very cool feature if the programmer uses
#+ variable names that make sense!
```

然后，运行 msgfmt。

`**msgfmt -o localized.sh.mo fr.po**`

将生成的 `localized.sh.mo` 文件放置在 `/usr/local/share/locale/fr/LC_MESSAGES` 目录中，并在脚本开头插入以下行：

```sh
TEXTDOMAINDIR=/usr/local/share/locale
TEXTDOMAIN=localized.sh
```

如果一个法语系统的用户运行该脚本，她将获得法语消息。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 在较旧的 Bash 或其他 shell 版本中，本地化需要使用 gettext，并使用 `-s` 选项。在这种情况下，脚本将变为：

&#124;  ```sh #!/bin/bash
# localized.sh

E_CDERROR=65

error() {
  local format=$1
  shift
  printf "$(gettext -s "$format")" "$@" >&2
  exit $E_CDERROR
}
cd $var &#124;&#124; error "Can't cd to %s." "$var"
read -p "$(gettext -s "Enter the value: ")" var
# ...
```  &#124;

|

需要设置并导出 `TEXTDOMAIN` 和 `TEXTDOMAINDIR` 变量到环境变量中。这应该在脚本内部完成。

---

本附录由 Stphane Chazelas 编写，Alfredo Pironti 提供了修改建议，以及 Bruno Haible（GNU gettext 的维护者）。
