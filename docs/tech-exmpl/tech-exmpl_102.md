# 求一个数字的各位数字之和程序

> 原文：[`techbyexample.com/add-all-digits-number-program/`](https://techbyexample.com/add-all-digits-number-program/)

## **概述**

目标是反复将一个数字的所有数字相加，直到结果只剩一个数字。

例如

```go
Input: 453
Step 1: 4+5+3 = 12
Step 2: 1+2 =3

Output: 3
```

另一个例子

```go
Input: 45
Step 1: 4+5 = 9

Output: 9
```

## **程序**

这是相同程序的代码

```go
package main

import "fmt"

func addDigits(num int) int {

	if num < 10 {
		return num
	}

	for num > 9 {
		num = sum(num)
	}

	return num

}

func sum(num int) int {
	output := 0
	for num > 0 {
		output = output + num%10
		num = num / 10
	}
	return output
}

func main() {
	output := addDigits(453)
	fmt.Println(output)

	output = addDigits(45)
	fmt.Println(output)

}
```

**输出**

```go
3
9
```
