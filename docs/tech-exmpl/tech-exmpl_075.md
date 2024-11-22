# 计算字符串中最后一个单词长度的程序

> 原文：[`techbyexample.com/program-length-last-word/`](https://techbyexample.com/program-length-last-word/)

## **概述**

目标是找到给定字符串中最后一个单词的长度

示例

```go
Input: "computer science"
Output: 7

The last word is science and its length is 7

Input: "computer science is a subject "
Output: 7

The last word is subject and ts length is 7

Input: " "
Output: 0

There is no last word hence answer is zero
```

## **程序**

下面是实现该功能的程序。

```go
package main

import "fmt"

func lengthOfLastWord(s string) int {
	lenS := len(s)

	lenLastWord := 0
	for i := lenS - 1; i >= 0; {
		for i >= 0 && string(s[i]) == " " {
			i--
		}
		if i < 0 {
			return 0
		}

		for i >= 0 && string(s[i]) != " " {
			//fmt.Println(i)
			//fmt.Println(string(s[i]))
			i--
			lenLastWord++
		}

		return lenLastWord
	}

	return 0
}

func main() {
	length := lengthOfLastWord("computer science")
	fmt.Println(length)

	length = lengthOfLastWord("computer science is a subject")
	fmt.Println(length)

	length = lengthOfLastWord("  ")
	fmt.Println(length)
}
```

**输出**

```go
7
7
0
```
