# 加一程序或向整数数组中加一

> 原文：[`techbyexample.com/plus-one-program/`](https://techbyexample.com/plus-one-program/)

## **概览**

给定一个整数数组。整体上，这个整数数组表示一个数字。假设该整数数组名为 digits，那么 digits[i]表示该整数的第 i 位数字。目标是将 1 加到这个整数数组中。必须在不将数组转换为整数类型 int 的情况下完成此操作。

示例

```go
Input: [1, 2]
Output: [1, 3]

Input: [9, 9]
Output: [1, 0, 0]
```

## **程序**

下面是该程序的代码。

```go
package main

import "fmt"

func plusOne(digits []int) []int {

	lenDigits := len(digits)

	output := make([]int, 0)

	lastDigit := digits[lenDigits-1]

	add := lastDigit + 1
	carry := add / 10

	lastDigit = add % 10
	output = append(output, lastDigit)

	for i := lenDigits - 2; i >= 0; i-- {
		o := digits[i] + carry
		lastDigit = o % 10
		carry = o / 10
		output = append(output, lastDigit)
	}

	if carry == 1 {
		output = append(output, 1)
	}

	return reverse(output, len(output))
}

func reverse(input []int, length int) []int {
	start := 0
	end := length - 1

	for start < end {
		input[start], input[end] = input[end], input[start]
		start++
		end--
	}

	return input
}

func main() {
	output := plusOne([]int{1, 2})
	fmt.Println(output)

	output = plusOne([]int{9, 9})
	fmt.Println(output)
}
```

**输出**

```go
[1 3]
[1 0 0]
```
