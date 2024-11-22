# 合并两个已排序数组的程序

> 原文：[`techbyexample.com/program-merge-two-sorted-arrays/`](https://techbyexample.com/program-merge-two-sorted-arrays/)

## **概述**

给定两个数组。两个数组都已经排序。

+   第一个数组的长度为 m+n

+   第二个数组的长度为 n

目标是合并这两个已排序的数组。第一个数组具有足够的长度，因此只应修改第一个数组。

```go
Input1: [2,3,4,0,0]
Input2: [1,5]
Output: [1, 2, 3, 4, 5]

Input1: [4,5,0,0,0,0]
Input2: [1, 2, 3, 7]
Output: [1, 2, 3, 4, 5, 7]
```

这里是我们可以采取的方法

+   将第一个数组中的所有元素按排序顺序移到末尾。第一个数组将变成

```go
[0,0,2,3,4]
```

+   现在从第一个数组的**mth**索引元素和第二个数组的**0th**索引开始。

+   比较两个数组，并将较小的元素放置在第一个数组的**0th**索引位置。第一个数组将变成

```go
[1, 0, 2, 3, 4]
```

+   重复这个过程。第一个数组末尾的值将在被覆盖之前移到前面，因为我们有足够的空间。

## **程序**

这里是实现该功能的程序。

```go
package main

import "fmt"

func merge(nums1 []int, m int, nums2 []int, n int) []int {

	if m == 0 {
		for k := 0; k < n; k++ {
			nums1[k] = nums2[k]
		}
		return nums1
	}
	nums1 = moveToEnd(nums1, m)
	i := n
	j := 0
	for k := 0; k < m+n; k++ {
		if i < m+n && j < n {
			if nums1[i] < nums2[j] {
				nums1[k] = nums1[i]
				i++
			} else {
				nums1[k] = nums2[j]
				j++
			}
		} else if j < n {
			nums1[k] = nums2[j]
			j++
		}

	}

	return nums1

}

func moveToEnd(nums []int, m int) []int {
	lenNums := len(nums)

	k := lenNums

	for i := m - 1; i >= 0; i-- {
		nums[k-1] = nums[i]
		k--
	}

	return nums
}

func main() {
	output := merge([]int{2, 3, 4, 0, 0}, 3, []int{1, 5}, 2)
	fmt.Println(output)

	output = merge([]int{4, 5, 0, 0, 0, 0}, 2, []int{1, 2, 3, 7}, 4)
	fmt.Println(output)
}
```

**输出**

```go
[1 2 3 4 5]
[1 2 3 4 5 7]
```
