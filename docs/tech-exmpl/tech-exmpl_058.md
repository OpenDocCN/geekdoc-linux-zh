# 数组中递增元素之间的最大差值

> 原文：[`techbyexample.com/maximum-difference-increasing-elements/`](https://techbyexample.com/maximum-difference-increasing-elements/)

## **概述**

给定一个数组。目标是找出两个索引 i 和 j 之间的最大差值，使得

+   j > i

+   arr[j] > arr[i]

如果不存在这样的索引，则返回 -1

示例 1

```go
Input: intervals = [8, 2, 6, 5]
Output: 4
Explanation: 6-2 = 4
```

示例 2

```go
Input: intervals = [8, 3, 2, 1]
Output: -1
Explanation: Condition is not satified
```

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func maximumDifference(nums []int) int {
	lenNums := len(nums)
	if lenNums == 0 || lenNums == 1 {
		return -1
	}
	minElement := nums[0]

	maxDifference := -1

	for i := 1; i < lenNums; i++ {
		diff := nums[i] - minElement
		if diff > maxDifference && diff != 0 {
			maxDifference = diff
		}

		if nums[i] < minElement {
			minElement = nums[i]
		}
	}

	return maxDifference
}

func main() {
	output := maximumDifference([]int{8, 2, 6, 5})
	fmt.Println(output)

	output = maximumDifference([]int{8, 3, 2, 1})
	fmt.Println(output)

}
```

**输出**

```go
4
-1
```
