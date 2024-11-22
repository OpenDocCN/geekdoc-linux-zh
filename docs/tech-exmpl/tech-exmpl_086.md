# 句子中的单词反转

> 原文：[`techbyexample.com/reverse-words-sentence/`](https://techbyexample.com/reverse-words-sentence/)

## **概述**

目标是反转给定句子中的单词

示例

```go
Input: "hello world"
Output: "word hello"
```

另一个例子。如果输入仅包含一个单词，那么该单词将被返回。

```go
Input: "hello"
Output: "hello"
```

这里是策略

+   首先，我们反转整个字符串。所以对于“hello world”，它变成了

```go
"dlrow olleh"
```

+   然后我们反转每个单词

```go
"world hello"
```

+   我们还需要注意去除开头或结尾的多余空格。

## **程序**

这是相同的程序。

```go
package main

import (
	"fmt"
	"regexp"
	"strings"
)

func reverseWords(s string) string {

	runeArray := []rune(s)
	length := len(runeArray)

	reverseRuneArray := reverse(runeArray)

	for i := 0; i < length; {
		for i < length && string(reverseRuneArray[i]) == " " {
			i++
		}
		if i == length {
			break
		}
		wordStart := i

		for i < length && string(reverseRuneArray[i]) != " " {
			i++
		}

		wordEnd := i - 1

		reverseRuneArray = reverseIndex(reverseRuneArray, wordStart, wordEnd)

	}

	noSpaceString := strings.TrimSpace(string(reverseRuneArray))
	space := regexp.MustCompile(`\s+`)
	return space.ReplaceAllString(noSpaceString, " ")
}

func reverse(s []rune) []rune {
	length := len(s)
	start := 0
	end := length - 1
	for start < end {
		s[start], s[end] = s[end], s[start]
		start++
		end--
	}
	return s
}

func reverseIndex(s []rune, i, j int) []rune {

	start := i
	end := j
	for start < end {
		s[start], s[end] = s[end], s[start]
		start++
		end--
	}
	return s
}

func main() {
	output := reverseWords("hello world")
	fmt.Println(output)

	output = reverseWords("hello")
	fmt.Println(output)
}
```

**输出**

```go
world hello
hello
```
