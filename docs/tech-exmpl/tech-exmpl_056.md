# 最大长度的连续子数组，包含相等数量的 0 和 1。

> 原文：[`techbyexample.com/max-length-array-zero-one/`](https://techbyexample.com/max-length-array-zero-one/)

## **概览**

给定一个数组，其中仅包含 0 和 1。目标是找到一个最大长度的子数组，包含相等数量的 0 和 1。让我们通过示例来理解。

示例 1

```go
Input: intervals = [0, 1]
Output: 2
```

示例 2

```go
Input: intervals = [0, 1, 1, 0, 1, 1]
Output: 4
```

下面是我们可以采用的方法

+   我们将创建一个数组 **leftSum**，其中 leftSum[i] 表示从索引 0 到 i 的数字之和。将 0 视为 -1，1 视为 1。

+   现在有两种情况。子数组要么从索引 0 开始，要么从其他非零的索引开始。

+   从左到右扫描 `leftSum` 数组。如果 `leftSum` 数组中某个索引的值为零，那么意味着子数组[0,i]中包含相等数量的 0 和 1。这将给出子数组从索引 0 开始的答案。

+   如果子数组不是从零开始的。再次扫描 `leftSum` 数组，找到索引 **i** 和 **j**，使得 **leftSum[i] == leftSum[j]**。为了找出这个情况，我们将使用一个映射。如果 j-i 的长度大于当前最大长度，则更新最大长度。

+   最后返回最大长度

## **程序**

这里是相应的程序。

```go
package main

import "fmt"

func findMaxLength(nums []int) int {
	lenNums := len(nums)

	if lenNums == 0 {
		return 0
	}

	currentSum := 0

	sumLeft := make([]int, lenNums)

	for i := 0; i < lenNums; i++ {
		if nums[i] == 0 {
			currentSum = currentSum - 1
		} else {
			currentSum = currentSum + 1
		}
		sumLeft[i] = currentSum
	}

	maxLength := 0

	max := 0
	min := 0

	for i := 0; i < lenNums; i++ {
		if sumLeft[i] == 0 {
			maxLength = i + 1
		}
		if sumLeft[i] > max {
			max = sumLeft[i]
		}

		if sumLeft[i] < min {
			min = sumLeft[i]
		}
	}

	numMap := make(map[int]int, max-min+1)

	for i := 0; i < lenNums; i++ {
		index, ok := numMap[sumLeft[i]]

		if ok {
			currentLength := i - index
			if currentLength > maxLength {
				maxLength = currentLength
			}
		} else {
			numMap[sumLeft[i]] = i
		}
	}

	return maxLength

}
func main() {
	output := findMaxLength([]int{0, 1})
	fmt.Println(output)

	output = findMaxLength([]int{0, 1, 1, 0, 1, 1})
	fmt.Println(output)
}
```

**输出**

```go
2
4
```
