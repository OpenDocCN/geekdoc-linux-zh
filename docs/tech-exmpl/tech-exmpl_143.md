# 如何在 Linux 上将命令运行在后台

> 原文：[`techbyexample.com/run-command-in-the-background-on-linux/`](https://techbyexample.com/run-command-in-the-background-on-linux/)

## **概述**

+   使用‘&’命令

+   使用‘bg’命令

+   使用‘nohup’工具

## **使用‘&’命令**

在命令末尾加上&以将命令发送到后台。

**语法**

```go
command &
```

**示例：**

```go
jhon-mbp:home jhon$ ping google.com > out 2>&1 &
[1] 17609
```

```go
jhon-mbp:home jhon$ ping google.com &
[1] 18223
PING google.com (172.217.167.206): 56 data bytes
```

**注意：**– 这很危险，因为它会将输出发送到终端，阻止你输入任何命令。因此，如果某个命令输出内容，请将其重定向到文件或其他地方，或者重定向到/dev/null，然后再将命令发送到后台。这就是为什么我将命令的输出重定向到名为‘out’的文件，并加上‘2>&1’将错误也重定向到文件中。

**jobs 命令**

它会列出所有在后台运行的作业，并在占位符[]中显示作业编号。

```go
jhon-mbp:home jhon$ jobs
[1]+  Running                 ping google.com > out 2>&1 &
```

**fg 命令**

**语法**：

```go
fg %job_number 
```

**注意：-** 你可以在运行命令时通过‘&’获取作业编号，也可以在‘jobs’命令的输出中找到它。

**示例：**

```go
jhon-mbp:home jhon$ fg %1
ping google.com > out 2>&1

^Cjhon-mbp:home jhon$ 
```

**注意：-** 一旦我们将上述命令带到前台，就必须使用‘Ctrl + C’取消它，因为它会阻塞终端。因此，始终保持另一个终端打开，以防某些操作未按计划执行，你可以使用**‘kill’或‘pkill’命令**从该终端终止该进程。

```go
kill -9 $PID_NUMBER
```

```go
pkill -9 -f "command name"
```

## **使用‘bg’命令**

要使用此命令，请按照以下两个步骤进行操作。

+   运行命令后按 Ctrl + Z 停止它

+   现在运行‘bg’命令，它将把上一个命令放到后台执行。

**示例：**

```go
./git-p4.py clone --p4_spec p4-specs/file-spec.json --repo /root/code/myapp/testrepo -v --revisions @2020/09/09,now
```

```go
Ctrl + Z #to Stop it
```

现在运行 bg 命令

```go
bg
```

## **使用‘nohup’工具**

只需在命令前加上‘nohup’前缀，它将把终端与进程分离，并将输出和错误信息发送到其默认日志文件，即当前目录中的 nohup.out。你可以像下面的命令一样配置输出文件。

**示例：**

```go
nohup ./git-p4.py clone --p4_spec p4-specs/file-spec.json --repo /root/code/myapp/testrepo -v --revisions @2020/09/09,now > results.out 2>&1 &
```

**注意：-**

+   在上述命令中，我们在最后加上‘&’，以释放当前会话中的终端，这样你就可以继续使用终端进行其他操作，而长时间运行的命令会继续在后台运行。

+   我们将输出/错误重定向到自定义文件，即通过在上述命令中使用这些行“**> results.out 2>&1**”将结果保存到 results.out。

+   我们通过在命令前加上‘nohup’，启动了‘workflow.py’脚本。
