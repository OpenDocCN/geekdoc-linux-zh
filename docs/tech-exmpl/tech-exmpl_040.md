# 最长连续序列程序

> 原文：[`techbyexample.com/longest-consecutive-sequence/`](https://techbyexample.com/longest-consecutive-sequence/)

# **概述**

给定一个整数数组，找出其中最长连续序列的长度。让我们通过一个例子来理解这个问题

**示例 1**

```go
Input: [4,7,3,8,2,1]
Output: 4
Reason: The longest consecutive sequence is [1,2,3,4]
```

**示例 2**

```go
Input: [4,7,3,8,2,1,9,24,10,11]
Output: 5
Reason: The longest consecutive sequence is [7,8,9,10,11]
```

朴素的思路是对数组进行排序，然后返回最长的连续序列。但我们可以在 O(n) 时间内完成此操作。思路是使用哈希表。以下是该思路

+   将每个数字存储在哈希表中。

+   然后从每个数字开始，找出以该数字为起点的最长连续序列的长度。如果一个数字假设为 n，我们尝试在哈希表中找到 n+1、n+2 …… 并得到从该数字开始的最长连续序列的长度

+   如果在步骤 2 中找到的长度大于之前找到的最长连续序列的长度，则更新最长连续序列的长度

# **程序**

以下是相同的程序

```go
package main

import "fmt"

func longestConsecutive(nums []int) int {
	numsMap := make(map[int]int)

	lenNums := len(nums)

	for i := 0; i < lenNums; i++ {
		numsMap[nums[i]] = 0
	}

	maxLCS := 0
	for i := 0; i < lenNums; i++ {
		currentLen := 1
		counter := 1

		for {
			val, ok := numsMap[nums[i]+counter]
			if ok {
				if val != 0 {
					currentLen += val
					break
				} else {
					currentLen += 1
					counter += 1
				}
			} else {
				break
			}
		}

		if currentLen > maxLCS {
			maxLCS = currentLen
		}
		numsMap[nums[i]] = currentLen
	}

	return maxLCS
}

func main() {
	output := longestConsecutive([]int{4, 7, 3, 8, 2, 1})
	fmt.Println(output)

	output = longestConsecutive([]int{4, 7, 3, 8, 2, 1, 9, 24, 10, 11})
	fmt.Println(output)

}
```

**输出：**

```go
4
5
```

此外，请查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
