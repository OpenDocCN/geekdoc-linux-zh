# 最大连续子数组和程序

> 原文：[`techbyexample.com/maximum-continuous-subarray-sum-program/`](https://techbyexample.com/maximum-continuous-subarray-sum-program/)

## **概述**

目标是找到给定输入数组中的最大子数组。子数组应该是连续的，并且至少包含一个元素

例如

```go
Input: [4, 5 ,-3]
Maximum Subarray is [4, 5]
Output: 9
```

另一个示例

```go
Input: [1, 2, -4, 4, 1]
Maximum Subarray is [4, 1]
Output: 5
```

我们在这里使用 Kadane 算法。在 Kadane 算法中，我们保持两个变量**max**和**currentMax**

+   **max**初始化为 IntMin，**currentMax**初始化为零

+   然后我们遍历数组中的每个元素

    +   设置**currentMax** = **currentMax** + a[i]

    +   如果**currentMax** > **max**，则将**max**设置为**currentMax**

    +   如果**currentMax**小于零，则**currentMax**被重置为零

## **程序**

这是相同的程序

```go
package main

import "fmt"

const (
	UintSize = 32 << (^uint(0) >> 32 & 1)
	MaxInt   = 1<<(UintSize-1) - 1 // 1<<31 - 1 or 1<<63 - 1
	MinInt   = -MaxInt - 1
)

func main() {
	input := []int{4, 5, -3}
	output := maxSubArray(input)
	fmt.Println(output)

	input = []int{1, 2, -4, 4, 1}
	output = maxSubArray(input)
	fmt.Println(output)
}

func maxSubArray(nums []int) int {
	lenNums := len(nums)

	currentMax := 0
	max := MinInt
	for i := 0; i < lenNums; i++ {
		currentMax = currentMax + nums[i]
		if currentMax > max {
			max = currentMax
		}

		if currentMax < 0 {
			currentMax = 0
		}

	}
	return max
}
```

**输出**

```go
9
5
```
