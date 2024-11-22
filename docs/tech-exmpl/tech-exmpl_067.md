# 查找数组中的所有重复项

> 原文：[`techbyexample.com/find-all-duplicates-array/`](https://techbyexample.com/find-all-duplicates-array/)

## **概述**

给定一个数组，其中所有元素的范围在[1, n]之间，n 为数组的长度。目标是找出该数组中的所有重复项

示例

```go
Input: [1, 2, 3, 2, 4, 3]
Output: [2, 3]
```

这里的思路是利用数字范围在[1, n]的事实。对于数组中的每个元素，将其索引处的值增加 n。所以

+   获取索引处的值时，我们使用 value%n

+   最终，如果任何索引处的值大于 2*n，那么它就是重复的。

## **程序**

这里是相应的程序。

```go
package main

import "fmt"

func findDuplicates(nums []int) []int {
	lenNums := len(nums)

	for i := 0; i < lenNums; i++ {
		index := (nums[i] - 1) % lenNums

		nums[index] = lenNums + nums[index]
	}

	k := 0

	for i := 0; i < lenNums; i++ {
		if nums[i] > 2*lenNums {
			nums[k] = i + 1
			k++
		}
	}

	return nums[0:k]

}

func main() {
	output := findDuplicates([]int{1, 2, 3, 2, 4, 3})
	fmt.Println(output)
}
```

**输出**

```go
[2 3]
```
