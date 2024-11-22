# 按频率排序字符

> 原文：[`techbyexample.com/sort-characters-frequency/`](https://techbyexample.com/sort-characters-frequency/)

# **概述**

给定一个输入字符串。目标是根据字符的频率对字符串进行排序。我们需要根据字符频率的降序进行排序。让我们通过一个示例来理解。

**示例 1**

```go
Input: "bcabcb"
Output: "bbbcca"
```

**示例 2**

```go
Input: "mniff"
Output: "ffmni"
```

# **程序**

以下是相同程序的代码

```go
package main

import (
	"fmt"
	"sort"
)

func frequencySort(s string) string {
	stringMap := make(map[byte]int)
	lenS := len(s)
	for i := 0; i < lenS; i++ {
		stringMap[s[i]]++
	}

	itemArray := make([]item, 0)

	for key, value := range stringMap {
		i := item{
			char:      key,
			frequency: value,
		}
		itemArray = append(itemArray, i)
	}

	sort.Slice(itemArray, func(i, j int) bool {
		return itemArray[i].frequency > itemArray[j].frequency
	})

	output := ""

	for i := 0; i < len(itemArray); i++ {
		for j := 0; j < itemArray[i].frequency; j++ {
			output = output + string(itemArray[i].char)
		}
	}

	return output

}

type item struct {
	char      byte
	frequency int
}

func main() {
	output := frequencySort("bcabcb")
	fmt.Println(output)

	output = frequencySort("mniff")
	fmt.Println(output)

}
```

**输出：**

```go
bbbcca
ffmni
```

另外，查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
