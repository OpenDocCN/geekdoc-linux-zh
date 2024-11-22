# 设计一个点击计数器

> 原文：[`techbyexample.com/system-design-hit-counter/`](https://techbyexample.com/system-design-hit-counter/)

# **概述**

目标是设计一个点击计数器，能够记录网站在过去 5 分钟内的访问次数。这是一个棘手的问题。你的设计应能够回答以下一般性查询

+   过去 n 分钟内的点击次数，其中 1 <= n <= 5

本教程中描述的解决方案也可以扩展到大于 5 分钟的情况。

思路是设计一个数据结构，能够记录过去五分钟内每一分钟的点击次数。

为此，我们可以使用以下数据结构

+   计数器数组 – 一个长度为 5 的整数数组。

+   时间戳数组 – 一个长度为 5 的整数数组。

Counter[i] 将存储第 i 分钟内的点击次数

Timestamp[i] 将存储第 i 分钟内最后一次点击的时间戳。它将是一个表示该分钟内的纪元时间戳的数字。

以下是算法

+   获取给定时间戳的纪元时间戳（以分钟为单位）。我们将其称为 **timestamp_epoch**

+   将该纪元时间戳除以 5 并取余。这个余数将被称为 **I**

+   如果 timestamp[i] = **timestamp_epoch**，那么这意味着第 i 分钟的上一条点击发生在同一分钟。因此，增加计数器。基本上执行 counter[i]++

+   如果 timestamp[i] != **timestamp_epoch**，那么这意味着第 i 分钟的最后一次点击发生在不同的分钟。因此，我们需要重置计数器。基本上执行 counter[i] = 1

# **程序**

以下是相应的代码

```go
package main

import "fmt"

var (
	counter   [5]int
	timestamp [5]int
)

func recordHit(timestamp_epoch int) {

	i := timestamp_epoch % 5

	if timestamp[i] != timestamp_epoch {
		counter[i] = 1
		timestamp[i] = timestamp_epoch
	} else {
		counter[i]++
	}

	fmt.Printf("After hit at time: %d\n", timestamp_epoch)
	fmt.Println(counter)
	fmt.Println(timestamp)
	fmt.Println()

}

func getNumberOfHit(timestamp_epoch int, minute int) (int, error) {

	if minute > 5 {
		return 0, fmt.Errorf("incorrect value of past minute passed")
	}
	hits := 0

	for k := timestamp_epoch; k > timestamp_epoch-minute; k-- {
		i := k % 5
		if timestamp[i] > timestamp_epoch-5 {
			hits = hits + counter[i]
		}
	}

	return hits, nil

}
func main() {
	recordHit(1000)

	recordHit(1000)

	hits, _ := getNumberOfHit(1000, 1)
	fmt.Printf("Current timestamp: %d,Last minue: %d,  Hits:%d\n\n", 1000, 1, hits)

	recordHit(1001)

	hits, _ = getNumberOfHit(1001, 2)
	fmt.Printf("Current timestamp: %d, Last minue: %d, Hits:%d\n\n", 1001, 2, hits)

	recordHit(1005)

	hits, _ = getNumberOfHit(1005, 5)
	fmt.Printf("Current timestamp: %d, Last minue: %d,  Hits:%d\n", 1005, 5, hits)

	hits, _ = getNumberOfHit(1005, 2)
	fmt.Printf("Current timestamp: %d, Last minue: %d, Hits:%d\n", 1005, 2, hits)

	recordHit(1007)
	hits, _ = getNumberOfHit(1007, 5)
	fmt.Printf("Current timestamp: %d, Last minue: %d, Hits:%d\n", 1007, 5, hits)

} 
```

**输出**

```go
After hit at time: 1000
[1 0 0 0 0]
[1000 0 0 0 0]

After hit at time: 1000
[2 0 0 0 0]
[1000 0 0 0 0]

Current timestamp: 1000,Last minue: 1,  Hits:2

After hit at time: 1001
[2 1 0 0 0]
[1000 1001 0 0 0]

Current timestamp: 1001, Last minue: 2, Hits:3

After hit at time: 1005
[1 1 0 0 0]
[1005 1001 0 0 0]

Current timestamp: 1005, Last minue: 5,  Hits:2
Current timestamp: 1005, Last minue: 2, Hits:1
After hit at time: 1007
[1 1 1 0 0]
[1005 1001 1007 0 0]

Current timestamp: 1007, Last minue: 5, Hits:2
```

让我们通过一个例子来理解它

+   在开始时，counter[i] = 0 对于 i = 0 到 4。同样，timestamp[i] = 0 对于 i = 0 到 4

```go
counter = [0, 0, 0, 0, 0]
timestamp = [0, 0, 0, 0, 0]
```

+   假设当前的纪元时间戳（以分钟为单位）是 1000。我们将其称为 **timestamp_epoch**

+   假设第一分钟有一次点击

    +   **i = 1000%5**，结果为 0

    +   **timestamp[i]** 为 0。因此，**timestamp[i]** 不等于 **timestamp_epoch**。

    +   执行 **counter[i]++**，这意味着 counter[0] = 1

    +   将 **timestamp[i]** 设置为 1000，即当前的分钟时间戳。因此，counter 和 timestamp 将会是

```go
counter = [1, 0, 0, 0, 0]
timestamp = [1000, 0, 0, 0, 0]
```

+   假设在第一分钟内有另一个点击

    +   i = 1000%5，结果为 0

    +   timestamp[i] 为 1。因此，timestamp[i] 等于 **timestamp_epoch**。

    +   执行 counter[i]++，这意味着 counter[0] = 2

    +   将 timestamp[0] 设置为 1000，即当前的分钟时间戳。因此，counter 和 timestamp 将会是

```go
counter = [2, 0, 0, 0, 0]
timestamp = [1000, 0, 0, 0, 0]
```

+   假设在第二分钟有另一次点击，当时该分钟的时间戳为 **1001**

    +   i = 1001%5，结果为 1

    +   timestamp[1] 当前为 0。因此，timestamp[1] 不等于 **timestamp_epoch**。

    +   执行 counter[1]++，这意味着 counter[1] = **timestamp_remainder**

    +   将 timestamp[1] 设置为 1001，即 **timestamp_epoch** 对应的分钟时间戳。因此，counter 和 timestamp 将会是

```go
counter = [2, 1, 0, 0, 0]
timestamp = [1000, 1001, 0, 0, 0]
```

+   现在假设在第 6 分钟又有一次点击。此时，当前的纪元时间戳（以分钟为单位）将是 1005。为什么是 1005？因为 1000 是第一分钟，那么 1005 就是第六分钟）

    +   i = 1005%5，结果为 0

    +   timestamp[0] 是 1000。所以 timestamp[0] 不等于 **timestamp_epoch**。

    +   重置计数器。将 counter[0] 设置为 1，这意味着 counter[0] = 1

    +   将 timestamp[0] 设置为 1005，这在分钟单位中是 **timestamp_epoch**。因此计数器和时间戳将是

```go
counter = [1, 1, 0, 0, 0]
timestamp = [1005, 1001, 0, 0, 0]
```

在任何给定时刻，要获取每分钟的点击次数，我们可以简单地将计数器数组中的所有条目加起来。

# **结论**

这就是设计 Hit Counter 的全部内容。希望你喜欢这篇文章。请在评论中分享反馈。
