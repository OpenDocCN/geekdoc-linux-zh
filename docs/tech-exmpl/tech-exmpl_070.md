# 搜索插入位置程序

> 原文：[`techbyexample.com/search-insert-position-program/`](https://techbyexample.com/search-insert-position-program/)

## **概述**

给定一个已排序的数组，其中包含不同的整数和一个目标值。目标是找到该目标值在数组中的插入位置。这里有两种情况

+   如果目标值存在于给定数组中，则返回该索引。

+   如果目标值在给定数组中不存在，返回它应该插入的位置

示例

```go
Input: [1,2,3]
Target Value: 4
Output: 3

Input: [1,2,3]
Target Value: 3
Output: 2
```

## **程序**

这是该程序的实现。

```go
package main

import "fmt"

func searchInsert(nums []int, target int) int {
	lenNums := len(nums)

	index := -1

	if target <= nums[0] {
		return 0
	}

	if target > nums[lenNums-1] {
		return lenNums
	}

	for i := 0; i < lenNums; i++ {
		if target <= nums[i] {
			index = i
			break
		}
	}

	return index

}

func main() {
	pos := searchInsert([]int{1, 2, 3}, 4)
	fmt.Println(pos)

	pos = searchInsert([]int{1, 2, 3}, 3)
	fmt.Println(pos)
}
```

**输出**

```go
3
2
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力通过示例覆盖所有概念。此教程适合那些希望掌握 Golang 并深入理解其知识的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)
