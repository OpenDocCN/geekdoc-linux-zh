# 检查 N 及其两倍是否存在

> 原文：[`techbyexample.com/number-double-exists/`](https://techbyexample.com/number-double-exists/)

# **概述**

给定一个数组。目标是查找是否存在一个数字，其两倍也存在于数组中。

**示例 1**

```go
Input: [8,5,4,3]
Output: true
Explanation: 4 and 8
```

**示例 2**

```go
Input: [1,3,7,9]
Output: false
Explanation: There exists no number for which its double exist
```

这里的思路是使用一个映射。对于每个元素，我们将检查并在满足条件时返回 true。

+   如果其两倍存在于映射中。

+   如果数字是偶数，那么检查其一半是否存在于映射中。

# **程序**

以下是实现该功能的程序

```go
package main

import "fmt"

func checkIfExist(arr []int) bool {
	numMap := make(map[int]bool)

	for i := 0; i < len(arr); i++ {
		if numMap[arr[i]*2] {
			return true
		}
		if arr[i]%2 == 0 && numMap[arr[i]/2] {
			return true
		}
		numMap[arr[i]] = true
	}
	return false
}

func main() {
	output := checkIfExist([]int{8, 5, 4, 3})
	fmt.Println(output)

	output = checkIfExist([]int{1, 3, 7, 9})
	fmt.Println(output)

}
```

**输出：**

```go
true
false
```

另外，请查看我们的系统设计教程系列：[系统设计教程系列](https://techbyexample.com/system-design-questions/)
