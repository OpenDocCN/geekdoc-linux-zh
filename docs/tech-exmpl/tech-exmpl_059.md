# 非重叠区间程序

> 原文：[`techbyexample.com/non-overlapping-intervals-program/`](https://techbyexample.com/non-overlapping-intervals-program/)

## **概述**

给定一个区间数组，其中 intervals[i] = [starti, endi]。我们需要找出最少需要移除的区间数量，以使得区间数组中的区间变得不重叠。

让我们通过一个例子来理解

```go
Input: intervals = [[2,3],[3,4],[4,5],[2,4]]
Output: 1
Explanation: [2,4] can be removed and the rest of the intervals are non-overlapping.
```

这个思路是先根据区间的开始时间排序，然后计算重叠的区间。

## **程序**

以下是相应的程序。

```go
package main

import (
	"fmt"
	"sort"
)

func eraseOverlapIntervals(intervals [][]int) int {
	lenIntervals := len(intervals)

	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})

	prevIntervalEnd := intervals[0][1]

	minIntervals := 0
	for i := 1; i < lenIntervals; i++ {
		currentIntervalStart := intervals[i][0]
		currentIntervalEnd := intervals[i][1]

		if currentIntervalStart < prevIntervalEnd {
			minIntervals++
			if prevIntervalEnd >= currentIntervalEnd {
				prevIntervalEnd = currentIntervalEnd
			}
		} else {
			prevIntervalEnd = currentIntervalEnd
		}
	}
	return minIntervals
}

func main() {

	output := eraseOverlapIntervals([][]int{{2, 3}, {3, 4}, {4, 5}, {2, 4}})
	fmt.Println(output)
}
```

**输出**

```go
6
13
```
