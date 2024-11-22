# 通配符匹配或正则表达式匹配问题

> [`techbyexample.com/wildcard-matching-problem/`](https://techbyexample.com/wildcard-matching-problem/)

## **概述**

我们给定一个输入正则表达式和输入字符串。正则表达式可以包含两个特殊字符

+   **星号‘*’ –** 星号匹配零个或多个字符。

+   **问号‘?’ –** 它匹配任何字符。

目标是判断给定的输入字符串是否匹配正则表达式。

例如

```go
Input String: aa
Regex Sring: aa
Output: true

Input String: ab
Regex Sring: a?
Output: true

Input String: aaaa
Regex Sring: *
Output: true

Input String: aa
Regex Sring: a
Output: false
```

以下是相同问题的递归解法

## **递归解法**

在递归解法中

+   如果遇到星号*，我们有两种情况。我们忽略模式中的*字符，继续处理模式中的下一个字符。另一种情况是我们在输入字符串中向前移动一个字符，假设*至少匹配一个字符。基本上，我们需要检查**(inputIndex, patternIndex+1)**和**(inputIndex+1, patternIndex)**是否匹配。如果其中一个返回 true，那么输入字符串匹配正则表达式。

+   如果遇到问号?，我们也简单地继续处理**(inputIndex+1, patternIndex+1)**

+   如果遇到一个简单字符，我们就简单地沿着输入字符串和模式继续前进，也就是说，我们继续处理**(inputIndex+1, patternIndex+1)**

这是程序

```go
package main

import "fmt"

func main() {
	output := isMatch("aa", "aa")
	fmt.Println(output)

	output = isMatch("aaaa", "*")
	fmt.Println(output)

	output = isMatch("ab", "a?")
	fmt.Println(output)

	output = isMatch("adceb", "*a*b")
	fmt.Println(output)

	output = isMatch("aa", "a")
	fmt.Println(output)

	output = isMatch("mississippi", "m??*ss*?i*pi")
	fmt.Println(output)

	output = isMatch("acdcb", "a*c?b")
	fmt.Println(output)
}

func isMatch(s string, p string) bool {
	runeInputArray := []rune(s)
	runePatternArray := []rune(p)
	if len(runeInputArray) > 0 && len(runePatternArray) > 0 {
		if runePatternArray[len(runePatternArray)-1] != '*' && runePatternArray[len(runePatternArray)-1] != '?' && runeInputArray[len(runeInputArray)-1] != runePatternArray[len(runePatternArray)-1] {
			return false
		}
	}
	return isMatchUtil([]rune(s), []rune(p), 0, 0, len([]rune(s)), len([]rune(p)))
}

func isMatchUtil(input, pattern []rune, inputIndex, patternIndex int, inputLength, patternLength int) bool {

	if inputIndex == inputLength && patternIndex == patternLength {
		return true
	} else if patternIndex == patternLength {
		return false
	} else if inputIndex == inputLength {
		if pattern[patternIndex] == '*' && restPatternStar(pattern, patternIndex+1, patternLength) {
			return true
		} else {
			return false
		}
	}

	if pattern[patternIndex] == '*' {
		return isMatchUtil(input, pattern, inputIndex, patternIndex+1, inputLength, patternLength) ||
			isMatchUtil(input, pattern, inputIndex+1, patternIndex, inputLength, patternLength)

	}

	if pattern[patternIndex] == '?' {
		return isMatchUtil(input, pattern, inputIndex+1, patternIndex+1, inputLength, patternLength)
	}

	if inputIndex < inputLength {
		if input[inputIndex] == pattern[patternIndex] {
			return isMatchUtil(input, pattern, inputIndex+1, patternIndex+1, inputLength, patternLength)
		} else {
			return false
		}
	}

	return false

}

func restPatternStar(pattern []rune, patternIndex int, patternLength int) bool {
	for patternIndex < patternLength {
		if pattern[patternIndex] != '*' {
			return false
		}
		patternIndex++
	}

	return true

}
```

**输出**

```go
true
true
true
true
false
false
false
```

## **动态规划解法**

上面的程序并不是一个优化的解法，因为子问题被一遍又一遍地解决。这个问题也可以通过动态规划（DP）来解决。

创建一个名为**isMatchingMatrix**的二维矩阵，其中

**isMatchingMatrix[i][j]** 如果输入字符串的前**i**个字符与模式的前**j**个字符匹配，则为 true

```go
If both input and pattern is empty
isMatchingMatrix[0][0] = true

If pattern is empty 
isMatchingMatrix[i][0] = fasle

If the input string is empty 
isMatchingMatrix[0][j] = isMatchingMatrix[0][j - 1] if pattern[j – 1] is '*'
```

以下是相同问题的程序。

```go
package main

import "fmt"

func main() {
	output := isMatch("aa", "aa")
	fmt.Println(output)

	output = isMatch("aaaa", "*")
	fmt.Println(output)

	output = isMatch("ab", "a?")
	fmt.Println(output)

	output = isMatch("adceb", "*a*b")
	fmt.Println(output)

	output = isMatch("aa", "a")
	fmt.Println(output)

	output = isMatch("mississippi", "m??*ss*?i*pi")
	fmt.Println(output)

	output = isMatch("acdcb", "a*c?b")
	fmt.Println(output)
}

func isMatch(s string, p string) bool {

	runeInput := []rune(s)
	runePattern := []rune(p)

	lenInput := len(runeInput)
	lenPattern := len(runePattern)

	isMatchingMatrix := make([][]bool, lenInput+1)

	for i := range isMatchingMatrix {
		isMatchingMatrix[i] = make([]bool, lenPattern+1)
	}

	isMatchingMatrix[0][0] = true
	for i := 1; i < lenInput; i++ {
		isMatchingMatrix[i][0] = false
	}

	if lenPattern > 0 {
		if runePattern[0] == '*' {
			isMatchingMatrix[0][1] = true
		}
	}

	for j := 2; j <= lenPattern; j++ {
		if runePattern[j-1] == '*' {
			isMatchingMatrix[0][j] = isMatchingMatrix[0][j-1]
		}

	}

	for i := 1; i <= lenInput; i++ {
		for j := 1; j <= lenPattern; j++ {

			if runePattern[j-1] == '*' {
				isMatchingMatrix[i][j] = isMatchingMatrix[i-1][j] || isMatchingMatrix[i][j-1]
			}

			if runePattern[j-1] == '?' || runeInput[i-1] == runePattern[j-1] {
				isMatchingMatrix[i][j] = isMatchingMatrix[i-1][j-1]
			}
		}
	}

	return isMatchingMatrix[lenInput][lenPattern]
}
```

**输出**

```go
true
true
true
true
false
false
false
```
