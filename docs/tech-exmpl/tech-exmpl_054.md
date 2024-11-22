# 在一组字符串中查找最长公共前缀

> 原文：[`techbyexample.com/longest-common-prefix-program/`](https://techbyexample.com/longest-common-prefix-program/)

## **概述**

给定一个字符串数组，目标是从该数组中找到最长公共前缀。如果没有公共前缀，应该输出一个空字符串。

示例 1

```go
Input: ["fan", "fat", "fame"]
Output: "fa"
```

示例 2

```go
Input: ["bat", "van", "cat"]
Output: ""
```

## **程序**

以下是相同的程序

```go
package main

import "fmt"

func longestCommonPrefix(strs []string) string {
	lenStrs := len(strs)

	if lenStrs == 0 {
		return ""
	}

	firstString := strs[0]

	lenFirstString := len(firstString)

	commonPrefix := ""
	for i := 0; i < lenFirstString; i++ {
		firstStringChar := string(firstString[i])
		match := true
		for j := 1; j < lenStrs; j++ {
			if (len(strs[j]) - 1) < i {
				match = false
				break
			}

			if string(strs[j][i]) != firstStringChar {
				match = false
				break
			}

		}

		if match {
			commonPrefix += firstStringChar
		} else {
			break
		}
	}

	return commonPrefix
}

func main() {
	output := longestCommonPrefix([]string{"fan", "fat", "fame"})
	fmt.Println(output)

	output = longestCommonPrefix([]string{"bat", "van", "cat"})
	fmt.Println(output)
}
```

**输出：**

```go
"fa"
""
```

**注意：** 另外，查看我们的系统设计教程系列：[系统设计教程系列](https://techbyexample.com/system-design-questions/)
