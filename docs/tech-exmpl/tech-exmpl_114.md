# 二进制加法程序

> 原文：[`techbyexample.com/add-two-binary-numbers-program/`](https://techbyexample.com/add-two-binary-numbers-program/)

## **概述**

目标是将两个给定的二进制数相加。二进制数仅由数字 0 和 1 组成。以下是每个数字的二进制加法逻辑

+   0+0 = 和为 0，进位为 0

+   0+1 = 和为 1，进位为 0

+   1+0 = 和为 0，进位为 0

+   1+1 = 和为 0，进位为 1

+   1+1+1 = 和为 1，进位为 1

示例

```go
Input: "101" + "11"
Output: "1000"

Input: "111" + "101"
Output: "1100"
```

## **程序**

以下是相应的程序

```go
package main

import (
	"fmt"
	"strconv"
)

func addBinary(a string, b string) string {
	lenA := len(a)
	lenB := len(b)

	i := lenA - 1
	j := lenB - 1

	var output string
	var sum int
	carry := 0
	for i >= 0 && j >= 0 {
		first := int(a[i] - '0')
		second := int(b[j] - '0')

		sum, carry = binarySum(first, second, carry)

		output = strconv.Itoa(sum) + output
		i = i - 1
		j = j - 1
	}

	for i >= 0 {
		first := int(a[i] - '0')

		sum, carry = binarySum(first, 0, carry)

		output = strconv.Itoa(sum) + output
		i = i - 1

	}

	for j >= 0 {
		second := int(b[j] - '0')

		sum, carry = binarySum(0, second, carry)

		output = strconv.Itoa(sum) + output
		j = j - 1
	}

	if carry > 0 {
		output = strconv.Itoa(1) + output
	}

	return output
}

func binarySum(a, b, carry int) (int, int) {
	output := a + b + carry

	if output == 0 {
		return 0, 0
	}

	if output == 1 {
		return 1, 0
	}

	if output == 2 {
		return 0, 1
	}

	if output == 3 {
		return 1, 1
	}

	return 0, 0
}

func main() {
	output := addBinary("101", "11")
	fmt.Println(output)

	output = addBinary("111", "101")
	fmt.Println(output)
}
```

**输出**

```go
1000
1100
```
