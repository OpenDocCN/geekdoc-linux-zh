# Golang 中的丑数 2 程序

> 原文：[`techbyexample.com/program-for-ugly-number-2-in-go-golang/`](https://techbyexample.com/program-for-ugly-number-2-in-go-golang/)

# **概述**

丑数是指其质因数仅限于 2、3 和 5 的数字。我们已经看到过一个程序，给定一个数字 n，判断它是否是丑数，如果是则返回 true，否则返回 false。以下是该程序的链接 [`techbyexample.com/program-ugly-number/`](https://techbyexample.com/program-ugly-number/)

在本教程中，我们将编写一个程序，给定一个数字 n，找到第 n 个丑数。

**示例 1**

```go
Input: 5
Output: 5
Reason: First five ugly numbers are 1, 2, 3, 4, 5 hence 5th ugly number is 5
```

**示例 2**

```go
Input: 20
Output: 36
```

这里的思路是使用动态规划。我们将追踪 2、3 和 5 的倍数。下一个丑数将始终是这三个数中的最小值。

# **程序**

以下是该程序：

```go
package main

import "fmt"

func nthUglyNumber(n int) int {
	dpUgly := make([]int, n)

	dpUgly[0] = 1

	next_multiple_of_2 := 2
	next_multiple_of_3 := 3
	next_multiple_of_5 := 5

	i2 := 0
	i3 := 0
	i5 := 0

	for i := 1; i < n; i++ {
		nextUglyNumber := minOfThree(next_multiple_of_2, next_multiple_of_3, next_multiple_of_5)
		dpUgly[i] = nextUglyNumber

		if nextUglyNumber == next_multiple_of_2 {
			i2++
			next_multiple_of_2 = 2 * dpUgly[i2]
		}

		if nextUglyNumber == next_multiple_of_3 {
			i3++
			next_multiple_of_3 = 3 * dpUgly[i3]
		}

		if nextUglyNumber == next_multiple_of_5 {
			i5++
			next_multiple_of_5 = 5 * dpUgly[i5]
		}

	}

	return dpUgly[n-1]

}

func minOfThree(a, b, c int) int {
	if a < b && a < c {
		return a
	}

	if b < c {
		return b
	}

	return c
}

func main() {
	output := nthUglyNumber(5)
	fmt.Println(output)

	output = nthUglyNumber(20)
	fmt.Println(output)
}
```

**输出**

```go
5
36
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们已尽力通过示例覆盖所有概念。本教程适用于那些希望掌握 Golang 并深入理解其内容的读者 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式，那么这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

同时，您还可以查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
