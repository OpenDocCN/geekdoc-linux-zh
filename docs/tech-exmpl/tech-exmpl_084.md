# 统计数字序列转换为字母的所有可能解码￼

> 原文：[`techbyexample.com/count-possible-decodings-digit/`](https://techbyexample.com/count-possible-decodings-digit/)

## **概述**

假设我们有下面的数字到字母的映射

```go
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

目标是统计给定数字序列的所有可能解码方式

示例

```go
Input: '15'
Output: 2

15 can be decoded as "AE" or "O"
```

## **程序**

这是相应的程序。

```go
package main

import (
	"fmt"
	"strconv"
)

func numDecodings(s string) int {

	runeArray := []rune(s)
	length := len(runeArray)
	if length == 0 {
		return 0
	}

	if string(runeArray[0]) == "0" {
		return 0
	}

	numwaysArray := make([]int, length)

	numwaysArray[0] = 1

	if length == 1 {
		return numwaysArray[0]
	}

	secondDigit := string(runeArray[0:2])
	num, _ := strconv.Atoi(secondDigit)
	if num > 10 && num <= 19 {
		numwaysArray[1] = 2
	} else if num > 20 && num <= 26 {
		numwaysArray[1] = 2
	} else if num == 10 || num == 20 {
		numwaysArray[1] = 1
	} else if num%10 == 0 {
		numwaysArray[1] = 0
	} else {
		numwaysArray[1] = 1
	}

	for i := 2; i < length; i++ {
		firstDigit := string(runeArray[i])
		if firstDigit != "0" {
			numwaysArray[i] = numwaysArray[i] + numwaysArray[i-1]
		}

		secondDigit := string(runeArray[i-1 : i+1])
		fmt.Println(i)
		fmt.Println(secondDigit)

		num, _ := strconv.Atoi(secondDigit)

		if num >= 10 && num <= 26 {
			numwaysArray[i] = numwaysArray[i] + numwaysArray[i-2]
		}
	}

	return numwaysArray[length-1]

}

func main() {
	output := numDecodings("15")
	fmt.Println(output)
}
```

**输出**

```go
2
```
