# 找出所有长度大于二的算术序列

> 原文：[`techbyexample.com/arithmetic-series/`](https://techbyexample.com/arithmetic-series/)

## **概述**

算术序列是一个每个元素之间的差相等的序列。在这个程序中，给定了一个整数数组。目标是找到所有长度大于二的算术序列。

这个问题最好通过一个例子来理解

示例

```go
Input: [2,3,4,5]
Output: 3
```

在上面的数组中，我们有三个长度大于 2 的算术切片

+   2,3,4

+   3,4,5

+   2,3,4,5

这是一个动态规划问题，因为它具有最优子结构。假设数组的名字是 input

+   dp[0] = 0

+   dp[1] = 0

+   dp[2] = 1 如果 dp[2] – dp[1] == dp[1] – dp[0]

+   dp[i] = 1 如果 dp[i] – dp[i-1] == dp[i-1] – dp[i-2]

其中 **dp[i]** 表示从长度大于 2 到长度 **i+1** 的算术子序列的数量

## **程序**

这里是相应的程序。

```go
package main

import "fmt"

func numberOfArithmeticSlices(nums []int) int {
	lenNums := len(nums)

	if lenNums <= 2 {
		return 0
	}

	dp := make([]int, lenNums)

	dp[0] = 0

	if (nums[2] - nums[1]) == nums[1]-nums[0] {
		dp[2] = 1
	}

	for i := 3; i < lenNums; i++ {
		if nums[i]-nums[i-1] == nums[i-1]-nums[i-2] {
			dp[i] = dp[i-1] + 1

		}
	}

	output := 0
	for i := 2; i < lenNums; i++ {
		output = output + dp[i]
	}

	return output

}

func main() {
	output := numberOfArithmeticSlices([]int{2, 3, 4, 5})
	fmt.Println(output)
}
```

**输出**

```go
3
```
