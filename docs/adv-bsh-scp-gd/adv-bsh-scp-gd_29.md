# 第二十四章。函数

> 原文：[`tldp.org/LDP/abs/html/functions.html`](https://tldp.org/LDP/abs/html/functions.html)

**目录**

24.1\. 复杂函数和函数复杂性

24.2\. 局部变量

24.3\. 无局部变量的递归

类似于“真实”的编程语言，Bash 有函数，尽管实现上有些受限。函数是一个子程序，一个实现一系列操作的 代码块，一个执行指定任务的“黑盒”。无论哪里有重复的代码，当任务在程序上只有细微的变化时，考虑使用函数。

**函数** `*function_name*` {

`*command*`...

}

或者

`*function_name*` () {

`*command*`...

}

这种第二种形式将使 C 语言程序员的内心感到欣慰（并且更可移植）。

与 C 语言一样，函数的起始括号可以可选地出现在第二行。

`*function_name*` ()

{

`*command*`...

}

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 函数可以被“压缩”成单行。

&#124;  ```sh fun () { echo "This is a function"; echo; }
#                                 ^     ^
```  &#124;

然而，在这种情况下，函数中的最后一个命令后面必须跟一个分号。

&#124;  ```sh fun () { echo "This is a function"; echo } # Error!
#                                       ^

fun2 () { echo "Even a single-command function? Yes!"; }
#                                                    ^
```  &#124;

|

函数通过调用其名称来调用，*触发*，函数调用等同于一个命令。

**示例 24-1。简单函数**

```sh
#!/bin/bash
# ex59.sh: Exercising functions (simple).

JUST_A_SECOND=1

funky ()
{ # This is about as simple as functions get.
  echo "This is a funky function."
  echo "Now exiting funky function."
} # Function declaration must precede call.

fun ()
{ # A somewhat more complex function.
  i=0
  REPEATS=30

  echo
  echo "And now the fun really begins."
  echo

  sleep $JUST_A_SECOND    # Hey, wait a second!
  while [ $i -lt $REPEATS ]
  do
    echo "----------FUNCTIONS---------->"
    echo "<------------ARE-------------"
    echo "<------------FUN------------>"
    echo
    let "i+=1"
  done
}

  # Now, call the functions.

funky
fun

exit $?
```

函数定义必须出现在对其第一次调用的前面。没有像 C 语言中那样的“声明”函数的方法。

```sh
f1
# Will give an error message, since function "f1" not yet defined.

declare -f f1      # This doesn't help either.
f1                 # Still an error message.

# However...

f1 ()
{
  echo "Calling function \"f2\" from within function \"f1\"."
  f2
}

f2 ()
{
  echo "Function \"f2\"."
}

f1  #  Function "f2" is not actually called until this point,
    #+ although it is referenced before its definition.
    #  This is permissible.

    # Thanks, S.C.
```

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 函数可能不为空！

&#124;  ```sh #!/bin/bash
# empty-function.sh

empty ()
{
}

exit 0  # Will not exit here!

# $ sh empty-function.sh
# empty-function.sh: line 6: syntax error near unexpected token `}'
# empty-function.sh: line 6: `}'

# $ echo $?
# 2

# Note that a function containing only comments is empty.

func ()
{
  # Comment 1.
  # Comment 2.
  # This is still an empty function.
  # Thank you, Mark Bova, for pointing this out.
}
# Results in same error message as above.

# However ...

not_quite_empty ()
{
  illegal_command
} #  A script containing this function will *not* bomb
  #+ as long as the function is not called.

not_empty ()
{
  :
} # Contains a : (null command), and this is okay.

# Thank you, Dominick Geyer and Thiemo Kellner.
```  &#124;

|

甚至可以在一个函数内部嵌套另一个函数，尽管这并不很有用。

```sh
f1 ()
{

  f2 () # nested
  {
    echo "Function \"f2\", inside \"f1\"."
  }

}  

f2  #  Gives an error message.
    #  Even a preceding "declare -f f2" wouldn't help.

echo    

f1  #  Does nothing, since calling "f1" does not automatically call "f2".
f2  #  Now, it's all right to call "f2",
    #+ since its definition has been made visible by calling "f1".

    # Thanks, S.C.
```

函数声明可以出现在不太可能的位置，甚至是在命令本应出现的地方。

```sh
ls -l &#124; foo() { echo "foo"; }  # Permissible, but useless.

if [ "$USER" = bozo ]
then
  bozo_greet ()   # Function definition embedded in an if/then construct.
  {
    echo "Hello, Bozo."
  }
fi  

bozo_greet        # Works only for Bozo, and other users get an error.

# Something like this might be useful in some contexts.
NO_EXIT=1   # Will enable function definition below.

[[ $NO_EXIT -eq 1 ]] && exit() { true; }     # Function definition in an "and-list".
# If $NO_EXIT is 1, declares "exit ()".
# This disables the "exit" builtin by aliasing it to "true".

exit  # Invokes "exit ()" function, not "exit" builtin.

# Or, similarly:
filename=file1

[ -f "$filename" ] &&
foo () { rm -f "$filename"; echo "File "$filename" deleted."; } &#124;&#124;
foo () { echo "File "$filename" not found."; touch bar; }

foo

# Thanks, S.C. and Christopher Head
```

函数名可以采取奇怪的形式。

```sh
  _(){ for i in {1..10}; do echo -n "$FUNCNAME"; done; echo; }
# ^^^         No space between function name and parentheses.
#             This doesn't always work. Why not?

# Now, let's invoke the function.
  _         # __________
#             ^^^^^^^^^^   10 underscores (10 x function name)!  
# A "naked" underscore is an acceptable function name.

# In fact, a colon is likewise an acceptable function name.

:(){ echo ":"; }; :

# Of what use is this?
# It's a devious way to obfuscate the code in a script.
```

参见示例 A-56

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 当脚本中出现同一函数的不同版本时会发生什么？

&#124;  ```sh #  As Yan Chen points out,
#  when a function is defined multiple times,
#  the final version is what is invoked.
#  This is not, however, particularly useful.

func ()
{
  echo "First version of func ()."
}

func ()
{
  echo "Second version of func ()."
}

func   # Second version of func ().

exit $?

#  It is even possible to use functions to override
#+ or preempt system commands.
#  Of course, this is *not* advisable.
```  &#124;

|
