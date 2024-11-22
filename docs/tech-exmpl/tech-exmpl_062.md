# 字符串或数组的不同（唯一）排列

> 原文：[`techbyexample.com/unique-permutations/`](https://techbyexample.com/unique-permutations/)

## **概述**

给定一个可以包含重复元素的整数数组，只找到所有不同的排列。

示例

```go
Input: [2, 2, 1]
Output: [[2 2 1] [2 1 2] [1 2 2]]

Input: [2, 2, 1, 1]
Output: [[2 2 1 1] [2 1 2 1] [2 1 1 2] [2 1 1 2] [2 1 2 1] [1 2 2 1] [1 2 1 2] [1 1 2 2] [1 2 1 2] [1 2 2 1] [1 1 2 2]]
```

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func permuteUnique(nums []int) [][]int {
	return permuteUtil(nums, 0, len(nums), len(nums))
}

func shouldSwap(nums []int, start, index int) bool {
	for i := start; i < index; i++ {
		if nums[start] == nums[index] {
			return false
		}

	}
	return true

}
func permuteUtil(nums []int, start, end int, length int) [][]int {
	output := make([][]int, 0)
	if start == end-1 {
		return [][]int{nums}
	} else {
		for i := start; i < end; i++ {
			if shouldSwap(nums, start, i) {
				nums[start], nums[i] = nums[i], nums[start]
				n := make([]int, length)
				for k := 0; k < length; k++ {
					n[k] = nums[k]
				}
				o := permuteUtil(n, start+1, end, length)
				output = append(output, o...)
				nums[i], nums[start] = nums[start], nums[i]
			}

		}
	}
	return output
}

func main() {
	output := permuteUnique([]int{2, 2, 1})
	fmt.Println(output)

	output = permuteUnique([]int{2, 2, 1, 1})
	fmt.Println(output)
}
```

**输出**

```go
[[2 2 1] [2 1 2] [1 2 2]]
[[2 2 1 1] [2 1 2 1] [2 1 1 2] [2 1 1 2] [2 1 2 1] [1 2 2 1] [1 2 1 2] [1 1 2 2] [1 2 1 2] [1 2 2 1] [1 1 2 2]]
```
