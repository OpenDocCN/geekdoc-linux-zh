# 通过删除找到字典中最长单词的程序

> 原文：[`techbyexample.com/longest-word-dictionary/`](https://techbyexample.com/longest-word-dictionary/)

# **概述**

给定一个字符串和一个单词字典。目标是找到字典中作为子序列出现在给定字符串中的最长单词。如果可能的结果数量超过 1，则返回字典序最小的最长单词。

**示例 1**

```go
s = "mbacnago", dictionary = ["ale","mango","monkey","plea"]
Output: "mango"
```

**示例 2**

```go
s = "mbacnago", dictionary = ["ba","ag"]
Output: "ag"
```

# **程序**

以下是相同程序的代码

```go
package main

import (
	"fmt"
	"sort"
)

func findLongestWord(s string, dictionary []string) string {
	sort.Slice(dictionary, func(i, j int) bool {
		lenI := len(dictionary[i])
		lenJ := len(dictionary[j])

		if lenI == lenJ {
			return dictionary[i] < dictionary[j]
		}

		return lenI > lenJ
	})

	lenS := len(s)

	for i := 0; i < len(dictionary); i++ {
		if isSubstring(s, dictionary[i], lenS) {
			return dictionary[i]
		}
	}

	return ""
}

func isSubstring(s string, sub string, lenS int) bool {
	lenSub := len(sub)

	if lenSub == 0 {
		return true
	}

	if lenSub > lenS {
		return false
	}

	for i, j := 0, 0; i < lenS && j < lenSub; {
		if i+lenSub-j-1 >= lenS {
			return false
		}
		if s[i] == sub[j] {
			j++
		}
		if j == lenSub {
			return true
		}
		i++
	}

	return false
}

func main() {
	output := findLongestWord("mbacnago", []string{"ale", "mango", "monkey", "plea"})
	fmt.Println(output)

	output = findLongestWord("mbacnago", []string{"ba", "ag"})
	fmt.Println(output)

}
```

**输出**

```go
mango
ag
```

此外，查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
