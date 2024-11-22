# 在数组中就地移除给定值的所有出现

> 原文：[`techbyexample.com/remove-element/`](https://techbyexample.com/remove-element/)

## **概述**

给定一个整数数组和一个目标元素。将数组中所有该目标元素的出现移除。移除操作必须在原地完成。

```go
Input: [1, 4, 2, 5, 4]
Target: 4
Output: [1, 2, 5]

Input: [1, 2, 3]
Target:3
Output: [1, 2]
```

## **程序**

这是相应的程序。

```go
package main

import (
	"fmt"
)

func removeElement(nums []int, val int) []int {
	lenNums := len(nums)

	k := 0

	for i := 0; i < lenNums; {
		if nums[i] != val {
			nums[k] = nums[i]
			k++
		}
		i++
	}
	return nums[0:k]
}

func main() {
	output := removeElement([]int{1, 4, 2, 5, 4}, 4)
	fmt.Println(output)

	output = removeElement([]int{1, 2, 3}, 3)
	fmt.Println(output)
}
```

**输出**

```go
[1 2 5]
[1 2]
```
