# 两栋最远的颜色不同的房子

> 原文：[`techbyexample.com/two-furthest-houses-different-colors/`](https://techbyexample.com/two-furthest-houses-different-colors/)

## **概述**

给定一个数组，表示房子的颜色。数组[i]表示索引 i 处的房子的颜色。目标是找到两栋最远的颜色不同的房子。

如果没有这样的索引存在，则返回 -1

示例 1

```go
Input: intervals = [2,2,2,1,2,2]
Output: 3
Explanation: House at index 0 and house at index 3 is of different colors
```

示例 2

```go
Input: intervals = [1, 2 ,3, 1, 2]
Output: 2
Explanation: House at index 0 and house at index 4 is of different colors
```

下面是我们可以采取的方法。

+   其中一栋房子将位于 0 索引或 n-1 索引处，其中 n 是数组的长度

+   我们可以首先假设 0 索引处的房子在解中，然后进行计算

+   我们可以接下来假设位于 n-1 索引的房子在解中，然后进行计算

## **程序**

以下是相应的程序。

```go
package main

import "fmt"

func maxDistance(colors []int) int {
	lenColors := len(colors)

	if lenColors == 0 || lenColors == 1 {
		return 0
	}
	maxDiff := 0

	leftColor := colors[0]
	rightColor := colors[lenColors-1]

	for i := 1; i < lenColors; i++ {
		if colors[i] != leftColor {
			maxDiff = i
		}
	}

	for i := lenColors - 2; i >= 0; i-- {
		if colors[i] != rightColor {
			diff := lenColors - i - 1
			if diff > maxDiff {
				maxDiff = diff
			}
		}
	}

	return maxDiff
}
func main() {
	output := maxDistance([]int{2, 2, 2, 1, 2, 2})
	fmt.Println(output)

	output = maxDistance([]int{1, 2, 3, 1, 2})
	fmt.Println(output)
}
```

**输出**

```go
3
4
```
