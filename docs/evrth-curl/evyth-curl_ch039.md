# 代码风格

具有共同样式的源代码比在不同地方使用不同样式的代码更容易阅读。这有助于使代码感觉像是一个连续的代码库。易于阅读是代码的一个重要属性，有助于在添加新内容时更容易进行代码审查，并在开发者试图弄清楚事情出错的原因时有助于调试代码。统一的样式比个人贡献者满足个人口味更重要。

我们的 C 代码有一些风格规则。其中大部分由`checksrc.pl`脚本验证和执行。可以通过`make checksrc`调用，或者在`./configure --enable-debug`被使用后，构建系统默认执行。

通常，遵循指南对任何人来说都不是问题，因为你只需要复制源代码中已经使用的样式，并且在我们的规则集中没有特别不寻常的规则。

我们还努力编写在所有主要平台以及尽可能多的平台上无警告的代码。显然会导致警告的代码不接受原样使用。

## 命名

尝试为你的新函数和变量名使用一个不令人困惑的命名方案。这并不意味着你必须在代码的其他地方使用相同的命名，只是名字应该是逻辑的、可理解的，并且根据它们的使用目的来命名。文件局部函数应该被定义为静态的。我们喜欢小写命名。

所有供公共使用的符号都必须以`curl`开头。全局内部符号以`Curl`开头。

## 缩进

我们只使用空格进行缩进，从不使用制表符。每个新的开括号使用两个空格。

```sh
if(something_is_true) {
  while(second_statement == fine) {
    moo();
  }
}
```

## 注释

由于我们编写的是 C89 代码，不允许使用`//`注释。它们直到 C99 标准中才被引入。我们只使用`/*注释*/`。

```sh
/* this is a comment */
```

## 长行

curl 中的源代码宽度不得超过 79 列。即使在现代大屏幕和高分辨率屏幕时代，保持这一规定的两个原因如下：

1.  较窄的列比宽列更容易阅读。这也是为什么报纸几十年来或几个世纪来都使用栏目的原因。

1.  较窄的列宽允许开发者更容易地在不同的窗口中并排查看多段代码。我经常在同一屏幕上并排打开两到三个源代码窗口，以及多个终端和调试窗口。

## 花括号

在 if/while/do/for 表达式中，我们将开括号写在关键字所在的同一行上，然后我们将闭合括号设置在初始关键字相同的缩进级别上。如下所示：

```sh
if(age < 40) {
  /* clearly a youngster */
}
```

如果括号中只包含一行语句，你可以省略括号：

```sh
if(!x)
  continue;
```

对于函数，开括号应该放在单独的一行上：

```sh
int main(int argc, char **argv)
{
  return 1;
}
```

## 在下一行使用 else

当使用花括号给条件表达式添加`else`子句时，我们在闭合花括号之后的新行上添加它。如下所示：

```sh
if(age < 40) {
  /* clearly a youngster */
}
else {
  /* probably intelligent */
}
```

## 括号前不加空格

当使用 if/while/do/for 编写表达式时，关键字与开括号之间不应有空格。例如：

```sh
while(1) {
  /* loop forever */
}
```

## 使用布尔条件

而不是在 if/while 条件中测试条件值，如布尔值与 TRUE 或 FALSE，指针与 NULL 或不为 NULL，整数与零或不为零，我们更喜欢：

```sh
result = do_something();
if(!result) {
  /* something went wrong */
  return result;
```

}

## 条件中不允许有赋值操作

为了提高可读性和减少条件语句的复杂性，我们避免在 if/while 条件中赋值变量。我们反对这种风格：

```sh
if((ptr = malloc(100)) == NULL)
  return NULL;
```

相反，我们鼓励使用更清晰的上述版本：

```sh
ptr = malloc(100);
if(!ptr)
  return NULL;
```

## 在新的一行上创建新的区块

我们从不将多个语句写在同一源行上，即使对于短的 if()条件也是如此。

```sh
if(a)
  return TRUE;
else if(b)
  return FALSE;
```

永远不要：

```sh
if(a) return TRUE;
else if(b) return FALSE;
```

## 操作符周围的空格

请在 C 表达式中操作符两侧使用空格。后缀**(), [], ->, ., ++, –**和一元**+, -, !, ~, &**操作符除外，它们不应有空格。

示例：

```sh
bla = func();
who = name[0];
age += 1;
true = !false;
size += -2 + 3 * (a + b);
ptr->member = a++;
struct.field = b--;
ptr = &address;
contents = *pointer;
complement = ~bits;
empty = (!*string) ? TRUE : FALSE;
```

## 返回值不需要括号

我们使用带括号的‘return’语句来返回值：

```sh
int works(void)
{
  return TRUE;
}
```

## sizeof 参数的括号

当在代码中使用 sizeof 运算符时，我们更喜欢将其参数用括号括起来：

```sh
int size = sizeof(int);
```

## 列对齐

有些语句不能在单行上完成，因为行会太长，语句难以阅读，或者由于其他风格指南。在这种情况下，语句跨越多行。

如果续行符是表达式或子表达式的部分，那么你应该在适当的列上对齐，以便容易判断它是语句的哪一部分。操作符不应开始续行符。在其他情况下，遵循 2 个空格缩进指南。以下是一些来自 libcurl 的示例：

```sh
if(Curl_pipeline_wanted(handle->multi, CURLPIPE_HTTP1) &&
   (handle->set.httpversion != CURL_HTTP_VERSION_1_0) &&
   (handle->set.httpreq == HTTPREQ_GET ||
    handle->set.httpreq == HTTPREQ_HEAD))
  /* did not ask for HTTP/1.0 and a GET or HEAD */
  return TRUE;
```

如果没有括号，使用默认缩进：

```sh
data->set.http_disable_hostname_check_before_authentication =
  (0 != va_arg(param, long)) ? TRUE : FALSE;
```

使用开括号进行函数调用：

```sh
if(option) {
  result = parse_login_details(option, strlen(option),
                               (userp ? &user : NULL),
                               (passwdp ? &passwd : NULL),
                               NULL);
}
```

与“当前打开”的括号对齐：

```sh
DEBUGF(infof(data, "Curl_pp_readresp_ %d bytes of trailing "
             "server response left\n",
             (int)clipamount));
```

## 平台相关代码

使用**#ifdef HAVE_FEATURE**来进行条件代码。我们避免在#ifdef 行中检查特定的操作系统或硬件。HAVE_FEATURE 应由 unix-like 系统的 configure 脚本来生成，而对于其他系统则硬编码在`config-[system].h`文件中。

我们还鼓励使用宏/函数，当 libcurl 构建时没有该功能，它们可能是空的或定义为常量，以使代码无缝。例如，**magic()**函数根据构建时的条件而有所不同：

```sh
#ifdef HAVE_MAGIC
void magic(int a)
{
  return a + 2;
}
#else
#define magic(x) 1
#endif

int content = magic(3);
```

## 不使用 typedef 的结构体

尽可能使用结构体，但不要为它们使用 typedef。使用`struct name`的方式来标识它们：

```sh
struct something {
   void *valid;
   size_t way_to_write;
};
struct something instance;
```

**不合适**：

```sh
typedef struct {
   void *wrong;
   size_t way_to_write;
} something;
something instance;
```
