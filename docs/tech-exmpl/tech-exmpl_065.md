# 数列中的第 n 位数字程序

> 原文：[`techbyexample.com/nth-digit-sequence-program/`](https://techbyexample.com/nth-digit-sequence-program/)

## **概述**

给定一个整数 n，找出无限序列 {1, 2, 3, 4 ….. 无限} 中的第 n 位数字。

示例

```go
Input: 14
Output: 1
```

数列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 … 的第 14 位是 1，它是数字 12 的一部分。

示例 2

```go
Input: 17
Output: 3
```

数列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13 … 的第 17 位是 3，它是数字 13 的一部分。

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func findNthDigit(n int) int {

	numDigits := 1
	tenIncrement := 1

	start := 1
	counter := 9 * tenIncrement * numDigits

	for n > counter {
		n = n - counter
		tenIncrement = tenIncrement * 10
		numDigits++
		start = start * 10
		counter = 9 * tenIncrement * numDigits
	}

	return findNthDigitUtil(start, numDigits, n)

}

func findNthDigitUtil(start, numDigits, n int) int {
	position := n % numDigits
	digitWhichHasN := 0
	if position == 0 {
		digitWhichHasN = start - 1 + n/numDigits
		return digitWhichHasN % 10
	} else {
		digitWhichHasN = start + n/numDigits
		positionFromBehind := numDigits - position
		answer := 0
		for positionFromBehind >= 0 {
			answer = digitWhichHasN % 10
			digitWhichHasN = digitWhichHasN / 10
			positionFromBehind--
		}
		return answer
	}

	return 0

}

func main() {
	output := findNthDigit(14)
	fmt.Println(output)

	output = findNthDigit(17)
	fmt.Println(output)
} 
```

**输出**

```go
1
3
```
