# 电话号码字母组合程序

> 原文：[`techbyexample.com/letter-combinations-phone/`](https://techbyexample.com/letter-combinations-phone/)

## **概述**

给定一个包含一些数字的输入字符串。数字与字母的映射方式类似于电话键盘上的映射

```go
2 = either "a", "b" or "c"
3 = either "d", "e" or "f"
4 = either "g", "h" or "i"
5 = either "j", "k" or "l"
6 = either "m", "n" or "co"
7 = either "p", "q" "r" or "s"
8 = either "t", "u" or "v"
9 = either "w", "x", "y" or "z"
```

给定一个只包含数字的输入字符串。目标是返回所有字母组合。

示例

```go
Input: "3"
Output: [d e f]

Input: "34"
Output: [dg dh di eg eh ei fg fh fi]

Input: "345"
Output: [dgj dgk dgl dhj dhk dhl dij dik dil egj egk egl ehj ehk ehl eij eik eil fgj fgk fgl fhj fhk fhl fij fik fil]
```

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func letterCombinations(digits string) []string {
	if digits == "" {
		return nil
	}
	letterMap := make(map[string][]string)

	letterMap["2"] = []string{"a", "b", "c"}
	letterMap["3"] = []string{"d", "e", "f"}
	letterMap["4"] = []string{"g", "h", "i"}
	letterMap["5"] = []string{"j", "k", "l"}
	letterMap["6"] = []string{"m", "n", "o"}
	letterMap["7"] = []string{"p", "q", "r", "s"}
	letterMap["8"] = []string{"t", "u", "v"}
	letterMap["9"] = []string{"w", "x", "y", "z"}

	runeDigits := []rune(digits)
	length := len(runeDigits)

	temp := ""

	return letterCombinationsUtil(runeDigits, 0, length, temp, letterMap)

}

func letterCombinationsUtil(runeDigits []rune, start, length int, temp string, letterMap map[string][]string) []string {

	if start == length {
		return []string{temp}
	}

	currentDigit := string(runeDigits[start])

	letters := letterMap[currentDigit]

	final := make([]string, 0)
	for _, val := range letters {
		t := temp + val
		output := letterCombinationsUtil(runeDigits, start+1, length, t, letterMap)
		final = append(final, output...)
	}
	return final
}

func main() {

	output := letterCombinations("3")
	fmt.Println(output)

	output = letterCombinations("34")
	fmt.Println(output)

	output = letterCombinations("345")
	fmt.Println(output)
}
```

**输出**

```go
[d e f]
[dg dh di eg eh ei fg fh fi]
[dgj dgk dgl dhj dhk dhl dij dik dil egj egk egl ehj ehk ehl eij eik eil fgj fgk fgl fhj fhk fhl fij fik fil]
```
