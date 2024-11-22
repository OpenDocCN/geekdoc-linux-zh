# 计算句子中单词总数的程序

> 原文：[`techbyexample.com/number-words-sentence-golang/`](https://techbyexample.com/number-words-sentence-golang/)

## **概述**

给定一个句子，计算其中的单词数。句子中的每个单词仅包含英文字符

示例

```go
Input: "Hello World"
Output: 2

Input: "This is hat"
Output: 3
```

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func countW(s string) int {

	lenS := len(s)
	numWords := 0

	for i := 0; i < lenS; {
		for i < lenS && string(s[i]) == " " {
			i++
		}

		if i < lenS {
			numWords++
		}

		for i < lenS && string(s[i]) != " " {
			i++
		}
	}

	return numWords
}

func main() {
	output := countW("Hello World")
	fmt.Println(output)

	output = countW("This is hat")
	fmt.Println(output)
}
```

**输出**

```go
2
3
```
