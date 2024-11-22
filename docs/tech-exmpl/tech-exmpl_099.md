# 最长递增子序列长度的程序

> 原文：[`techbyexample.com/longest-increasing-subsequence/`](https://techbyexample.com/longest-increasing-subsequence/)

## **概述**

目标是找到给定输入数组中的最长递增子序列。它是一个给定序列中的最长子序列，且每个元素都大于其前一个元素

例如

```go
Input: [1,5,7,6]
The longest subsequence is [1,5,6] which is of length 3
Output: 3
```

另一个例子

```go
Input: [3,2,1]
The longest subsequence is either {3}, {2} or {1}. Each is of length 1
Output: 1
```

最长递增子序列是一个动态规划问题。假设输入数组名为**input**。假设**lis**是一个数组，其中**lis[i]**表示索引**i**处的最长递增子序列的长度。

然后

+   **lis[0]** = 1

+   **lis[i]** = **max(lis[j])** + 1，其中 0 <= **j** < **i** 且 **input[i]** > **input[j]**

+   如果没有这样的**j**，则**lis[i]** = 1

## **程序**

这是相同问题的程序。

```go
package main

import "fmt"

func lengthOfLIS(nums []int) int {

	lenNums := len(nums)
	lis := make([]int, lenNums)

	for i := 0; i < lenNums; i++ {
		lis[i] = 1
	}

	for i := 1; i < lenNums; i++ {
		for j := 0; j < i; j++ {
			if nums[i] > nums[j] && lis[i] < (lis[j]+1) {
				lis[i] = lis[j] + 1
			}
		}
	}

	max := 0

	for i := 0; i < lenNums; i++ {
		if lis[i] > max {
			max = lis[i]
		}
	}

	return max
}

func main() {
	output := lengthOfLIS([]int{1, 5, 7, 6})
	fmt.Println(output)

	output = lengthOfLIS([]int{3, 2, 1})
	fmt.Println(output)
}
```

**输出**

```go
3
1
```
