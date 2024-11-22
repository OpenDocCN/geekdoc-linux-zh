# 引爆最大炸弹程序。

> 原文：[`techbyexample.com/detonate-maximum-bombs-program/`](https://techbyexample.com/detonate-maximum-bombs-program/)

# **概述**

给定一个二维数组，其中每个数组项有三个值。

+   i – 表示炸弹的 x 坐标。

+   j – 表示炸弹的 y 坐标。

+   r – 表示炸弹的爆炸半径。

一个炸弹在爆炸时会引发其范围内所有炸弹的爆炸。当这些炸弹爆炸时，它们将再次引发其范围内所有炸弹的爆炸。

你只能引爆一个炸弹。这个问题的思路是找出可以引爆的最大炸弹数量。

**示例 1**

```go
Input: [[1,0,3],[5,0,4]]
Output: 2
```

第一个炸弹在第二个炸弹的爆炸范围内。所以当我们引爆第二个炸弹时，第二个炸弹和第一个炸弹都会被引爆。

**示例 2**

```go
Input:  [[2,2,2],[10,10,5]]
Output: 1
```

这两个炸弹彼此不在对方的爆炸范围内。

解决这个问题的思路是将所有内容视为一个有向图，如果第二个炸弹在第一个炸弹的爆炸范围内，则存在从第一个炸弹指向第二个炸弹的有向边。

一旦构建了这个图，我们可以从每个节点做深度优先搜索（DFS），以获得它能引爆的最大炸弹数。我们还会存储之前计算的结果。

# **程序**

以下是相应的程序。

```go
package main

import (
	"fmt"
	"math"
)

func maximumDetonation(bombs [][]int) int {

	if len(bombs) == 0 {
		return 0
	}
	max := 1
	detonationMap := make(map[int][]int)

	for i := 0; i < len(bombs); i++ {
		for j := 0; j < len(bombs); j++ {
			if i != j {
				if float64(bombs[i][2]) >= distance(bombs[i], bombs[j]) {
					if arr, ok := detonationMap[i]; ok {
						arr = append(arr, j)
						detonationMap[i] = arr
					} else {
						var arr []int
						arr = append(arr, j)
						detonationMap[i] = arr
					}
				}
			}
		}
	}

	for key := range detonationMap {
		detonated := 1
		queue := []int{key}
		visited := make(map[int]bool)
		visited[key] = true

		for len(queue) > 0 {
			cur := queue[0]
			queue = queue[1:]

			for _, val := range detonationMap[cur] {
				if !visited[val] {
					detonated++
					visited[val] = true
					queue = append(queue, val)
				}
			}
		}
		if detonated == len(bombs) {
			return len(bombs)
		}

		if detonated > max {
			max = detonated
		}
	}
	return max
}

func distance(a []int, b []int) float64 {
	ret := math.Sqrt(math.Pow(float64(a[0]-b[0]), 2) + math.Pow(float64(a[1]-b[1]), 2))
	return ret
}

func main() {
	output := maximumDetonation([][]int{{1, 0, 3}, {5, 0, 4}})
	fmt.Println(output)

	output = maximumDetonation([][]int{{2, 2, 2}, {10, 10, 5}})
	fmt.Println(output)

}
```

**输出：**

```go
2
1
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，并且我们尽力通过实例覆盖所有概念。本教程适合那些希望深入掌握并精通 Golang 的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

同时，也可以查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
