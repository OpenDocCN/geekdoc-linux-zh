# 腐烂橙子程序

> 原文：[`techbyexample.com/rotting-oranges-program/`](https://techbyexample.com/rotting-oranges-program/)

# **概述**

给定一个 m*n 的矩阵，每个条目包含三个值。

+   0 – 表示该条目为空

+   1 – 表示该条目包含新鲜的橙子

+   2 – 表示该条目包含腐烂的橙子

一只腐烂的橙子将在 1 天内腐烂相邻的橙子。对于给定的橙子，任何位于其上、下、左、右的橙子都是相邻橙子。对角线上的橙子不算在内。

目标是找到所有橙子腐烂所需的天数。如果所有橙子无法腐烂，则输出 -1。如果一个新鲜橙子无法从腐烂橙子传播到达，就会发生这种情况。

**示例 1**

```go
Input: [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```

顶部有一个腐烂的橙子，它将腐烂其相邻的两个橙子。这些腐烂的相邻橙子会进一步腐烂它们的相邻橙子。

**示例 2**

```go
Input: [[1,1]]
Output: -1
```

# **程序**

以下是相同程序的代码

```go
package main

import "fmt"

func orangesRotting(grid [][]int) int {
	numRows := len(grid)
	numColumns := len(grid[0])

	queue := make([][2]int, 0)

	for i := 0; i < numRows; i++ {
		for j := 0; j < numColumns; j++ {
			if grid[i][j] == 2 {
				queue = append(queue, [2]int{i, j})
			}
		}
	}

	neighboursIndex := [][2]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}
	numMinutes := 0
	for {
		n := len(queue)
		for i := 0; i < n; i++ {
			pop := queue[0]
			queue = queue[1:]

			a := pop[0]
			b := pop[1]
			for k := 0; k < 4; k++ {
				neighbourX := a + neighboursIndex[k][0]
				neighbourY := b + neighboursIndex[k][1]

				if isValid(neighbourX, neighbourY, numRows, numColumns) {
					if grid[neighbourX][neighbourY] == 1 {
						grid[neighbourX][neighbourY] = 2
						queue = append(queue, [2]int{neighbourX, neighbourY})
					}
				}

			}
		}

		if len(queue) == 0 {
			break
		}
		numMinutes++
	}

	for i := 0; i < numRows; i++ {
		for j := 0; j < numColumns; j++ {
			if grid[i][j] == 1 {
				return -1
			}
		}
	}

	return numMinutes
}

func isValid(i, j, numRows, numColumns int) bool {
	if i >= numRows || i < 0 {
		return false
	}

	if j >= numColumns || j < 0 {
		return false
	}
	return true
}

func main() {
	output := orangesRotting([][]int{{2, 1, 1}, {1, 1, 0}, {0, 1, 1}})
	fmt.Println(output)

	output = orangesRotting([][]int{{1, 1}})
	fmt.Println(output)
}
```

**输出：**

```go
4
-1
```

此外，请查看我们的系统设计教程系列：[系统设计教程系列](https://techbyexample.com/system-design-questions/)
