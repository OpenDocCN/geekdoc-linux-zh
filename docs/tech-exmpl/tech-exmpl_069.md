# 乘法运算程序：两个字符串的乘法

> 原文：[`techbyexample.com/program-multiply-two-strings/`](https://techbyexample.com/program-multiply-two-strings/)

## **概述**

编写一个程序来乘以两个字符串。

示例

```go
Input: "12"*"12"
Output: 144

Input: "123"*"12"
Output: 1476
```

## **程序**

以下是相应的程序。

```go
package main

import (
	"fmt"
	"math"
	"strconv"
)

func multiply(num1 string, num2 string) string {

	if len(num1) > len(num2) {
		num2, num1 = num1, num2
	}

	output := 0

	k := 0

	carry := 0

	for i := len(num1) - 1; i >= 0; i-- {

		x := 0
		temp := 0
		for j := len(num2) - 1; j >= 0; j-- {

			digit1, _ := strconv.Atoi(string(num1[i]))
			digit2, _ := strconv.Atoi(string(num2[j]))

			multiply_output := digit1*digit2 + carry

			carry = multiply_output / 10

			temp = multiply_output%10*int(math.Pow(10, float64(x))) + temp
			x = x + 1
		}

		temp = carry*int(math.Pow(10, float64(x))) + temp
		carry = 0

		output = temp*int(math.Pow(10, float64(k))) + output
		k = k + 1
	}

	return strconv.Itoa(output)
}

func main() {
	output := multiply("12", "12")
	fmt.Println(output)

	output = multiply("123", "12")
	fmt.Println(output)
}
```

**输出**

```go
144
1476
```
