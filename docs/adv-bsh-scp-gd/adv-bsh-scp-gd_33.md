# 第二十八章. 间接引用

> 原文：[`tldp.org/LDP/abs/html/ivr.html`](https://tldp.org/LDP/abs/html/ivr.html)

我们已经看到，引用一个变量，`$var`，可以获取其*值*。但是，关于*值的值*呢？`$$var`又是怎么回事？

实际的表示法是`*\$$var*`，通常在 eval（有时是 echo）之前。这被称为*间接引用*。

**示例 28-1. 间接变量引用**

```sh
#!/bin/bash
# ind-ref.sh: Indirect variable referencing.
# Accessing the contents of the contents of a variable.

# First, let's fool around a little.

var=23

echo "\$var   = $var"           # $var   = 23
# So far, everything as expected. But ...

echo "\$\$var  = $$var"         # $$var  = 4570var
#  Not useful ...
#  \$\$ expanded to PID of the script
#  -- refer to the entry on the $$ variable --
#+ and "var" is echoed as plain text.
#  (Thank you, Jakob Bohm, for pointing this out.)

echo "\\\$\$var = \$$var"       # \$$var = $23
#  As expected. The first $ is escaped and pasted on to
#+ the value of var ($var = 23 ).
#  Meaningful, but still not useful. 

# Now, let's start over and do it the right way.

# ============================================== #

a=letter_of_alphabet   # Variable "a" holds the name of another variable.
letter_of_alphabet=z

echo

# Direct reference.
echo "a = $a"          # a = letter_of_alphabet

# Indirect reference.
  eval a=\$$a
# ^^^        Forcing an eval(uation), and ...
#        ^   Escaping the first $ ...
# ------------------------------------------------------------------------
# The 'eval' forces an update of $a, sets it to the updated value of \$$a.
# So, we see why 'eval' so often shows up in indirect reference notation.
# ------------------------------------------------------------------------
  echo "Now a = $a"    # Now a = z

echo

# Now, let's try changing the second-order reference.

t=table_cell_3
table_cell_3=24
echo "\"table_cell_3\" = $table_cell_3"            # "table_cell_3" = 24
echo -n "dereferenced \"t\" = "; eval echo \$$t    # dereferenced "t" = 24
# In this simple case, the following also works (why?).
#         eval t=\$$t; echo "\"t\" = $t"

echo

t=table_cell_3
NEW_VAL=387
table_cell_3=$NEW_VAL
echo "Changing value of \"table_cell_3\" to $NEW_VAL."
echo "\"table_cell_3\" now $table_cell_3"
echo -n "dereferenced \"t\" now "; eval echo \$$t
# "eval" takes the two arguments "echo" and "\$$t" (set equal to $table_cell_3)

echo

# (Thanks, Stephane Chazelas, for clearing up the above behavior.)

#   A more straightforward method is the ${!t} notation, discussed in the
#+ "Bash, version 2" section.
#   See also ex78.sh.

exit 0
```

|

在 Bash 中，间接引用是一个多步骤的过程。首先，取一个变量的名称：`varname`。然后，引用它：`$varname`。接着，引用引用：`$$varname`。然后，*转义*第一个$：`\$$varname`。最后，强制重新评估表达式并分配它：**eval newvar=\$$varname**。

|

变量的间接引用在实践中有何用途？它给 Bash 带来了一点点类似*指针*的功能，例如在 C 语言中的表格查找。它还有一些其他非常有趣的应用……

Nils Radtke 展示了如何构建“动态”变量名并评估其内容。这在源文件配置文件时非常有用。

```sh
#!/bin/bash

# ---------------------------------------------
# This could be "sourced" from a separate file.
isdnMyProviderRemoteNet=172.16.0.100
isdnYourProviderRemoteNet=10.0.0.10
isdnOnlineService="MyProvider"
# ---------------------------------------------

remoteNet=$(eval "echo \$$(echo isdn${isdnOnlineService}RemoteNet)")
remoteNet=$(eval "echo \$$(echo isdnMyProviderRemoteNet)")
remoteNet=$(eval "echo \$isdnMyProviderRemoteNet")
remoteNet=$(eval "echo $isdnMyProviderRemoteNet")

echo "$remoteNet"    # 172.16.0.100

# ================================================================

#  And, it gets even better.

#  Consider the following snippet given a variable named getSparc,
#+ but no such variable getIa64:

chkMirrorArchs () { 
  arch="$1";
  if [ "$(eval "echo \${$(echo get$(echo -ne $arch &#124;
       sed 's/^\(.\).*/\1/g' &#124; tr 'a-z' 'A-Z'; echo $arch &#124;
       sed 's/^.\(.*\)/\1/g')):-false}")" = true ]
  then
     return 0;
  else
     return 1;
  fi;
}

getSparc="true"
unset getIa64
chkMirrorArchs sparc
echo $?        # 0
               # True

chkMirrorArchs Ia64
echo $?        # 1
               # False

# Notes:
# -----
# Even the to-be-substituted variable name part is built explicitly.
# The parameters to the chkMirrorArchs calls are all lower case.
# The variable name is composed of two parts: "get" and "Sparc" . . .
```

**示例 28-2. 将间接引用传递给*awk*** 

```sh
#!/bin/bash

#  Another version of the "column totaler" script
#+ that adds up a specified column (of numbers) in the target file.
#  This one uses indirect references.

ARGS=2
E_WRONGARGS=85

if [ $# -ne "$ARGS" ] # Check for proper number of command-line args.
then
   echo "Usage: `basename $0` filename column-number"
   exit $E_WRONGARGS
fi

filename=$1         # Name of file to operate on.
column_number=$2    # Which column to total up.

#===== Same as original script, up to this point =====#

# A multi-line awk script is invoked by
#   awk "
#   ...
#   ...
#   ...
#   "

# Begin awk script.
# -------------------------------------------------
awk "

{ total += \$${column_number} # Indirect reference
}
END {
     print total
     }

     " "$filename"
# Note that awk doesn't need an eval preceding \$$.
# -------------------------------------------------
# End awk script.

#  Indirect variable reference avoids the hassles
#+ of referencing a shell variable within the embedded awk script.
#  Thanks, Stephane Chazelas.

exit $?
```
| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 这种间接引用的方法有点棘手。如果二阶变量改变了它的值，那么一阶变量必须正确地取消引用（如上例所示）。幸运的是，Bash 版本 2 中引入的`*${!variable}*`表示法（参见示例 37-2 和示例 A-22）使间接引用更加直观。 |

|

Bash 不支持指针算术，这严重限制了间接引用的实用性。实际上，在脚本语言中的间接引用，最多只是一种事后考虑。

|
