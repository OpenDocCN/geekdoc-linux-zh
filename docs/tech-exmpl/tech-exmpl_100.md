# 检查给定的两个字符串是否是字谜

> 原文：[`techbyexample.com/anagrams-two-string/`](https://techbyexample.com/anagrams-two-string/)

## **概述**

**字谜** 是通过重新排列一个单词或短语中的字母，通常使用原始字母且每个字母只使用一次，形成的一个新单词或短语。例如，单词 *anagram* 本身可以重新排列成 *nagaram*，同样，单词 *binary* 可以变成 *brainy*^，而单词 *adobe* 可以变成 *abode*。

例如

```go
Input: abc, bac
Ouput: true

Input: abc, cb
Ouput: false
```

这是实现这个的思路。创建一个字符串到整数的映射。现在

+   遍历第一个字符串，并增加映射中每个字符的计数

+   遍历第二个字符串，并减少映射中每个字符的计数

+   再次遍历第一个字符串，如果映射中某个字符的计数不为零，则返回 false。

+   最后返回 true

## **程序**

这是相应的程序代码。

```go
package main

import "fmt"

func isAnagram(s string, t string) bool {

	lenS := len(s)
	lenT := len(t)

	if lenS != lenT {
		return false
	}

	anagramMap := make(map[string]int)

	for i := 0; i < lenS; i++ {
		anagramMap[string(s[i])]++
	}

	for i := 0; i < lenT; i++ {
		anagramMap[string(t[i])]--
	}

	for i := 0; i < lenS; i++ {
		if anagramMap[string(s[i])] != 0 {
			return false
		}
	}

	return true
}

func main() {
	output := isAnagram("abc", "bac")
	fmt.Println(output)

	output = isAnagram("abc", "bc")
	fmt.Println(output)
}
```

**输出**

```go
true
false
```
