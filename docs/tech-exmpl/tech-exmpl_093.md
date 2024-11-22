# 程序用于查找给定子字符串是否存在

> 原文：[`techbyexample.com/program-substring-exists/`](https://techbyexample.com/program-substring-exists/)

## **概述**

在本教程中，我们将看到一种最简单的方法来查找给定字符串中的子字符串。请注意，这可能不是最有效的策略。

策略将是

+   匹配给定字符串中从每个索引开始的子字符串

这种方法的整体时间复杂度将是 **O(mn)**，其中 m 是子字符串的长度，**n**是输入字符串的大小

我们的程序将返回给定子字符串在原始字符串中开始的索引。如果子字符串不存在于给定字符串中，程序将返回-1。

示例

```go
Input: "lion"
Substring: "io"
Output: 1
```

**“io”** 在 **“lion”** 中的位置为 1

另一个例子

**“tus”** 在 **“tub”** 中不存在

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func strStr(haystack string, needle string) int {
	runeHayStack := []rune(haystack)
	runeNeedle := []rune(needle)
	lenHayStack := len(runeHayStack)
	lenNeedle := len(runeNeedle)

	if lenNeedle == 0 && lenHayStack == 0 {
		return 0
	}

	if lenNeedle > lenHayStack {
		return -1
	}

	for i := 0; i <= lenHayStack-lenNeedle; i++ {
		k := i
		j := 0

		for j < lenNeedle {
			if runeHayStack[k] == runeNeedle[j] {
				k = k + 1
				j = j + 1
			} else {
				break
			}
		}

		if j == lenNeedle {
			return i
		}
	}

	return -1

}

func main() {
	output := strStr("lion", "io")
	fmt.Println(output)

	output = strStr("tub", "tus")
	fmt.Println(output)
}
```

**输出**

```go
1
-1
```

**注意：** 查看我们的 Golang 进阶教程。该系列教程内容详尽，我们尽力通过示例覆盖所有概念。本教程适合那些希望获得 Golang 专业知识并深入理解的人——[Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章适合你——[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)
