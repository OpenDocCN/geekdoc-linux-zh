# 区间和数组程序

> 原文：[`techbyexample.com/range-sum-array-program/`](https://techbyexample.com/range-sum-array-program/)

## **概述**

给定了一个数字数组。目标是找到该数组中的区间和。这意味着会给出一个区间，其中包含左索引和右索引。我们需要在给定的数字数组中找到左索引和右索引之间的总和。

看起来很简单，对吧？只需从给定数组的左索引遍历到右索引并返回总和。但问题在于，允许的时间复杂度是 O(1)。

这里是我们可以遵循的方法，以便能够在 O(1) 时间复杂度内返回答案。

+   从给定的数字数组中预先计算出另一个 sum_array。

+   sum_array[i] = 从索引 0 到索引 i 的数字之和。

+   对于给定的区间，包含左索引和右索引，只需返回 sum_array[left-1] – sum_array[right]。当然，我们需要验证 left-1 是否大于或等于零。

## **程序**

这是相应的程序。

```go
package main

import "fmt"

type NumArray struct {
	sum_nums []int
}

func initNumArray(nums []int) NumArray {
	length := len(nums)
	sum_nums := make([]int, length)

	sum_nums[0] = nums[0]

	for i := 1; i < length; i++ {
		sum_nums[i] = nums[i] + sum_nums[i-1]
	}

	return NumArray{
		sum_nums: sum_nums,
	}
}

func (this *NumArray) SumRange(left int, right int) int {
	leftSum := 0
	if left > 0 {
		leftSum = this.sum_nums[left-1]
	}
	return this.sum_nums[right] - leftSum
}

func main() {
	nums := []int{1, 3, 5, 6, 2}
	na := initNumArray(nums)

	output := na.SumRange(2, 4)
	fmt.Println(output)

}
```

**输出**

```go
13
```
