# 用于从已排序数组中移除重复项的程序

> 原文：[`techbyexample.com/program-remove-duplicates-sorted-array/`](https://techbyexample.com/program-remove-duplicates-sorted-array/)

## **概述**

目标是从已排序的数组中移除重复项。

**示例**

```go
Input: [1, 1, 1, 2]
Output: [1, 2]

Input: [1, 2, 3, 3]
Output: [1, 2, 3]
```

## **程序**

以下是相同功能的程序

```go
package main

import "fmt"

func main() {
	input := []int{1, 1, 1, 2}
	output := removeDuplicates(input)
	fmt.Println(output)

	input = []int{1, 2, 3, 3}
	output = removeDuplicates(input)
	fmt.Println(output)
}

func removeDuplicates(nums []int) []int {
	lenArray := len(nums)

	k := 0
	for i := 0; i < lenArray; {
		nums[k] = nums[i]
		k++
		for i+1 < lenArray && nums[i] == nums[i+1] {
			i++
		}
		i++
	}

	return nums[0:k]
}
```

**输出**

```go
[1 2]
[1 2 3]
```
