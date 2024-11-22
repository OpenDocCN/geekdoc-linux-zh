# 给定整数数组的幂集程序

> 原文：[`techbyexample.com/program-for-power-set-array/`](https://techbyexample.com/program-for-power-set-array/)

## **概述**

给定一个包含所有唯一元素的整数数组。目标是返回该数组的幂集

```go
Input: [1, 2]
Output: [[],[1],[2],[1,2]]

Input: [1, 2, 3]
Output: [[] [1] [2] [1 2] [3] [1 3] [2 3] [1 2 3]]
```

如果给定数组中的元素个数为 n，那么幂集中的元素个数将是 pow(2, n)。假设 n 是 3，那么幂集中的元素个数将是 pow(2, n)=8

假设我们取从 0 到（8-1）之间的所有二进制转换，即从 0 到 7。

```go
000
001
010
011
100
101
110
111
```

上面每个二进制数字代表一个幂集

例如

```go
000 - []
001 - [1]
010 - [2]
011 - [1, 2]
100 - [3]
101 - [1, 3]
110 - [2, 3]
111 - [1, 2, 3]
```

## **程序**

这是相应的程序。

```go
package main

import (
	"fmt"
	"math"
)

func subsets(nums []int) [][]int {

	lengthNums := len(nums)
	powerSetLength := int(math.Pow(2, float64(lengthNums)))
	output := make([][]int, 0)

	for i := 0; i < powerSetLength; i++ {

		result := make([]int, 0)
		for j := 0; j < lengthNums; j++ {
			val := int(i) & int(1<<j)
			if val != 0 {
				result = append(result, nums[j])
			}
		}

		output = append(output, result)
	}

	return output
}

func main() {
	output := subsets([]int{1, 2})
	fmt.Println(output)

	output = subsets([]int{1, 2, 3})
	fmt.Println(output)
}
```

**输出**

```go
[[] [1] [2] [1 2]]
[[] [1] [2] [1 2] [3] [1 3] [2 3] [1 2 3]]
```
