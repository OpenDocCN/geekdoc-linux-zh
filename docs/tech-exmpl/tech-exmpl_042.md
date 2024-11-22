# 程序：反转字符串中的元音字母

> 原文：[`techbyexample.com/reverse-vowels-of-a-string/`](https://techbyexample.com/reverse-vowels-of-a-string/)

# **概述**

给定一个字符串。目标是反转该字符串中的所有元音字母

**示例 1**

```go
Input: "simple"
Output: "sempli"
```

**示例 2**

```go
Input: "complex"
Output: "cemplox"
```

# **程序**

以下是相同的程序

```go
package main

import "fmt"

func reverseVowels(s string) string {
	runeS := []rune(s)
	lenS := len(runeS)

	for i, j := 0, lenS-1; i < j; {
		for i < j {
			if !vowel(runeS[i]) {
				i++
			} else {
				break
			}
		}
		if i == j {
			break
		}
		for i < j {
			if !vowel(runeS[j]) {
				j--
			} else {
				break
			}
		}

		if i == j {
			break
		}

		runeS[i], runeS[j] = runeS[j], runeS[i]
		i++
		j--

	}

	return string(runeS)

}

func vowel(s rune) bool {
	if s == 'a' || s == 'e' || s == 'i' || s == 'o' || s == 'u' {
		return true
	}

	if s == 'A' || s == 'E' || s == 'I' || s == 'O' || s == 'U' {
		return true
	}

	return false
}

func main() {
	output := reverseVowels("simple")
	fmt.Println(output)

	output = reverseVowels("complex")
	fmt.Println(output)

}
```

**输出：**

```go
sempli
cemplox
```

另外，请查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
