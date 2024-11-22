# 字符串交错程序

> 原文：[`techbyexample.com/interleaving-string-program/`](https://techbyexample.com/interleaving-string-program/)

**概述**

给定三个字符串**s1**、**s2**、**s3**。判断字符串**s3**是否是字符串的交错组合。

如果满足以下条件，**s3**将是字符串**s1**和**s2**的交错字符串。

+   **s3**包含**s1**和**s2**的所有字符，并且每个字符串中的字符顺序保持不变。

示例

```go
s1: aabcc
s2: dbbca
s3: aadbbcbcac

Output: true
```

## **递归解法**

以下是相同问题的递归解法

```go
package main

import "fmt"

func main() {
	output := isInterleave("aabcc", "dbbca", "aadbbcbcac")
	fmt.Println(output)

	output = isInterleave("", "", "")
	fmt.Println(output)
}
func isInterleave(s1 string, s2 string, s3 string) bool {
	s1Rune := []rune(s1)
	s2Rune := []rune(s2)
	s3Rune := []rune(s3)

	lenS1 := len(s1Rune)
	lenS2 := len(s2Rune)
	lenS3 := len(s3Rune)

	if lenS1+lenS2 != lenS3 {
		return false
	}

	return isInterleaveUtil(s1Rune, s2Rune, s3Rune, 0, 0, 0, lenS1, lenS2, lenS3)
}

func isInterleaveUtil(s1, s2, s3 []rune, x, y, z, lenS1, lenS2, lenS3 int) bool {
	if x == lenS1 && y == lenS2 && z == lenS3 {
		return true
	}

	if x < lenS1 && z < lenS3 && s1[x] == s3[z] {
		match := isInterleaveUtil(s1, s2, s3, x+1, y, z+1, lenS1, lenS2, lenS3)
		if match {
			return true
		}
	}

	if y < lenS2 && z < lenS3 && s2[y] == s3[z] {
		return isInterleaveUtil(s1, s2, s3, x, y+1, z+1, lenS1, lenS2, lenS3)

	}
	return false
}
```

**输出**

```go
true
true
```

如果你注意到上面的程序，许多子问题被反复计算，因此该解法的复杂度是指数级的。我们可以在这里使用动态规划来降低整体时间复杂度。

这是相同问题的程序

## **动态规划解法**

```go
package main

import "fmt"

func main() {
	output := isInterleave("aabcc", "dbbca", "aadbbcbcac")
	fmt.Println(output)

	output = isInterleave("", "", "")
	fmt.Println(output)
}
func isInterleave(s1 string, s2 string, s3 string) bool {
	s1Rune := []rune(s1)
	s2Rune := []rune(s2)
	s3Rune := []rune(s3)

	lenS1 := len(s1Rune)
	lenS2 := len(s2Rune)
	lenS3 := len(s3Rune)

	if lenS1+lenS2 != lenS3 {
		return false
	}

	interleavingMatrix := make([][]bool, lenS1+1)

	for i := range interleavingMatrix {
		interleavingMatrix[i] = make([]bool, lenS2+1)
	}

	i := 1
	k := 1

	interleavingMatrix[0][0] = true

	for i <= lenS1 && k <= lenS3 {
		if s1Rune[i-1] == s3Rune[k-1] {
			interleavingMatrix[i][0] = true
			i++
			k++
		} else {
			break
		}
	}

	i = 1
	k = 1

	for i <= lenS2 && k <= lenS3 {
		if s2Rune[i-1] == s3Rune[k-1] {
			interleavingMatrix[0][i] = true
			i++
			k++
		} else {
			break
		}
	}

	for i := 1; i <= lenS1; i++ {
		for j := 1; j <= lenS2; j++ {

			if s1Rune[i-1] == s3Rune[i+j-1] {
				interleavingMatrix[i][j] = interleavingMatrix[i-1][j]
			}

			if s2Rune[j-1] == s3Rune[i+j-1] && !interleavingMatrix[i][j] {
				interleavingMatrix[i][j] = interleavingMatrix[i][j-1]
			}

		}
	}

	return interleavingMatrix[lenS1][lenS2]
}
```

**输出**

```go
true
true
```
