# 排序数组中的二分查找程序

> 原文：[`techbyexample.com/program-binary-search/`](https://techbyexample.com/program-binary-search/)

## **概述**

思路是对给定的目标元素在输入数组中进行二分查找。如果目标元素存在，则输出其索引。如果目标元素不存在，则输出 -1。

预期时间复杂度为 O(logn)

**示例 1**

```go
Input: [1, 4, 5, 6]
Target Element: 4
Output: 1

Target element 4 is present at index 1
```

**示例 2**

```go
Input: [1, 2, 3]
Target Element: 4
Output: -1

Target element 4 is present at index 1
```

## **程序**

```go
package main

import "fmt"

func search(nums []int, target int) int {
	return binarySearch(nums, 0, len(nums)-1, target)
}

func binarySearch(nums []int, start, end, target int) int {
	if start > end {
		return -1
	}
	mid := (start + end) / 2

	if nums[mid] == target {
		return mid
	}

	if target < nums[mid] {
		return binarySearch(nums, start, mid-1, target)
	} else {
		return binarySearch(nums, mid+1, end, target)
	}
}

func main() {
	index := search([]int{1, 4, 5, 6}, 4)
	fmt.Println(index)

	index = search([]int{1, 2, 3, 6}, 4)
	fmt.Println(index)
}
```

**输出**

```go
1
-1
```

另请查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
