# 程序：在不使用乘法或除法运算符的情况下，除以两个整数

> 原文：[`techbyexample.com/divide-two-integers-progam/`](https://techbyexample.com/divide-two-integers-progam/)

## **概述**

给定两个数字，目标是将这两个数字相除并返回商。在解法中忽略余数。但是必须在不使用乘法或除法运算符的情况下进行除法运算。

+   第一个数字是被除数

+   第二个数字是除数

例如

```go
Input: 15,2
Ouput: 7

Input: -15,2
Ouput: -7

Input: 15,-2
Ouput: -7

Input: -15,-2
Ouput: 7
```

这里是实现方法的思路。首先要注意的是

+   如果被除数和除数都是正数或都是负数，则商是正数

+   如果被除数和除数中有一个是负数，则商是负数

因此，被除数和除数的符号之间有一个异或关系。我们可以按照以下步骤编写程序

+   首先，根据上面的异或逻辑确定商的符号。

+   然后将被除数和除数都变为正数。

+   现在将除数加倍，直到它小于或等于被除数。同时，为每次增量保持一个计数器

+   counter*sign 将是答案

## **程序**

这是相应的程序。

```go
package main

import (
	"fmt"
	"math"
)

func divide(dividend int, divisor int) int {

	sign := 1
	if dividend < 0 || divisor < 0 {
		sign = -1
	}

	if dividend < 0 && divisor < 0 {
		sign = 1
	}

	if dividend < 0 {
		dividend = -1 * dividend
	}

	if divisor < 0 {
		divisor = -1 * divisor
	}

	start := divisor

	i := 0

	for start <= dividend {
		start = start + divisor
		i++
	}

	output := i * sign

	return output
}

func main() {
	output := divide(15, 2)
	fmt.Println(output)

	output = divide(-15, 2)
	fmt.Println(output)

	output = divide(15, -2)
	fmt.Println(output)

	output = divide(-15, -2)
	fmt.Println(output)
}
```

**输出**

```go
7
-7
-7
7
```
