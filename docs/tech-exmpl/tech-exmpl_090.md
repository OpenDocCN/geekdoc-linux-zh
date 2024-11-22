# 计算一个数的幂的程序

> 原文：[`techbyexample.com/program-power-number/`](https://techbyexample.com/program-power-number/)

## **概述**

目标是计算给定整数的幂。将有两个输入

+   数字本身 — 数字可以是正数也可以是负数，还可以是浮动数

+   幂 — 幂可以是正数也可以是负数

示例

```go
Input: Num:2, Power:4
Output: 16

Input: Num:2, Power:-4
Output: 0.0625
```

## **程序**

这里是相应的程序。

```go
package main

import "fmt"

func pow(x float64, n int) float64 {

	if x == 0 {
		return 0
	}

	if n == 0 {
		return 1
	}

	if n == 1 {
		return x
	}

	if n == -1 {
		return 1 / x
	}

	val := pow(x, n/2)

	m := x
	if n < 0 {
		m = 1 / x
	}

	if n%2 == 1 || n%2 == -1 {
		return val * val * m
	} else {
		return val * val
	}

}

func main() {
	output := pow(2, 4)
	fmt.Println(output)

	output = pow(2, -4)
	fmt.Println(output)
}
```

**输出**

```go
16
0.0625
```
