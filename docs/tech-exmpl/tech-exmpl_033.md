# 丑数的程序

> 原文：[`techbyexample.com/program-ugly-number/`](https://techbyexample.com/program-ugly-number/)

# **概述**

丑数是其质因数仅限于 2、3 和 5 的数字。

给定一个数字 n，如果它是丑数，则返回 true，否则返回 false。

**示例 1**

```go
Input: 12
Output: true
```

**示例 2**

```go
Input: 7 
Output: false
```

思路是

+   当数字是 2 的因数时，不断将数字除以 2

+   当数字是 3 的因数时，不断将数字除以 3

+   当数字是 5 的因数时，不断将数字除以 5

最后，如果数字等于 1，则返回 true，否则返回 false

# **程序**

以下是相应的程序

```go
package main

import "fmt"

func isUgly(n int) bool {
	if n == 0 {
		return false
	}
	if n == 1 {
		return true
	}

	for {
		if n%2 != 0 {
			break
		}
		n = n / 2
	}

	for {
		if n%3 != 0 {
			break
		}
		n = n / 3
	}

	for {
		if n%5 != 0 {
			break
		}
		n = n / 5
	}

	return n == 1
}

func main() {
	output := isUgly(12)
	fmt.Println(output)

	output = isUgly(7)
	fmt.Println(output)
}
```

**输出**

```go
true
false
```

另外，查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
