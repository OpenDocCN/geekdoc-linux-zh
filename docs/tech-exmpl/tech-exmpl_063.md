# 一组句子中的最大单词数

> 原文：[`techbyexample.com/maximum-words-group-sentences/`](https://techbyexample.com/maximum-words-group-sentences/)

## **概述**

给定一组句子。找出在该组句子中出现最多单词的句子。

示例

```go
Input: ["Hello World", "This is hat]
Output: 3

Input: ["Hello World", "This is hat", "The cat is brown"]
Output: 4
```

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func mostWordsFound(sentences []string) int {
	lenSentences := len(sentences)

	max := 0
	for i := 0; i < lenSentences; i++ {
		countWord := countW(sentences[i])
		if countWord > max {
			max = countWord
		}
	}

	return max
}

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
	output := mostWordsFound([]string{"Hello World", "This is hat"})
	fmt.Println(output)

	output = mostWordsFound([]string{"Hello World", "This is hat", "The cat is brown"})
	fmt.Println(output)
}
```

**输出**

```go
3
4
```
