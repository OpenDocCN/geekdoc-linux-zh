# `lscpu`命令

在 Linux/Unix 中，`lscpu`用于显示 CPU 架构信息。`lscpu`从`sysfs`和`/proc/cpuinfo`文件收集 CPU 架构信息。

例如：

```sh
      manish@godsmack:~$ lscpu
   Architecture:        x86_64
   CPU op-mode(s):      32-bit, 64-bit
   Byte Order:          Little Endian
   CPU(s):              4
   On-line CPU(s) list: 0-3
   Thread(s) per core:  2
   Core(s) per socket:  2
   Socket(s):           1
   NUMA node(s):        1
   Vendor ID:           GenuineIntel
   CPU family:          6
   Model:               142
   Model name:          Intel(R) Core(TM) i5-7200U CPU @ 2.50GHz
   Stepping:            9
   CPU MHz:             700.024
   CPU max MHz:         3100.0000
   CPU min MHz:         400.0000
   BogoMIPS:            5399.81
   Virtualization:      VT-x   
   L1d cache:           32K
   L1i cache:           32K
   L2 cache:            256K
   L3 cache:            3072K
   NUMA node0 CPU(s):   0-3 

```

## 选项

`-a, --all` 在输出中包含在线和离线 CPU 的行（-e 的默认值）。此选项只能与选项-e 或-p 一起指定。例如：`lsof -a`

`-b, --online` 将输出限制为在线 CPU（-p 的默认值）。此选项只能与选项-e 或-p 一起指定。例如：`lscpu -b`

`-c, --offline` 将输出限制为离线 CPU。此选项只能与选项-e 或-p 一起指定。

`-e, --extended [=list]` 以人类可读的格式显示 CPU 信息。例如：`lsof -e`

更多信息：使用`man lscpu`或`lscpu --help`
