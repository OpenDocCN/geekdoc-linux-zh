# 最小路径和程序

> 原文：[`techbyexample.com/minimum-path-sum-program/`](https://techbyexample.com/minimum-path-sum-program/)

## **概述**

有一个 m*n 的矩阵，包含非负整数。目标是找到一条从左上角到右下角的最小路径和。你只能向右或向下移动。

例如，假设我们有如下矩阵

![](img/695599be49b8f5c57183e0e8a42623c8.png)

然后，最小路径和如下图所示。它的和为 1+1+2+2+1 = 7

```go
[{0,0}, {1,0}, {1,1}, {2,1}, {2,2}
```

这是一个动态规划问题，因为它具有最优子结构。假设矩阵的名称是 input

+   minPath[0][0] = input[0][0]

+   minPath[i][j] = ming(minPath[i-1][j], minPath[i][j-1])) + input[i][j]

其中 minPath[i][j] 表示从 {0,0} 到 {i,j} 的最小路径和

## **程序**

以下是相应的程序。

```go
package main

import "fmt"

func minPathSum(grid [][]int) int {
	rows := len(grid)
	columns := len(grid[0])
	sums := make([][]int, rows)

	for i := 0; i < rows; i++ {
		sums[i] = make([]int, columns)
	}

	sums[0][0] = grid[0][0]

	for i := 1; i < rows; i++ {
		sums[i][0] = grid[i][0] + sums[i-1][0]
	}

	for i := 1; i < columns; i++ {
		sums[0][i] = grid[0][i] + sums[0][i-1]
	}

	for i := 1; i < rows; i++ {
		for j := 1; j < columns; j++ {
			if sums[i-1][j] < sums[i][j-1] {
				sums[i][j] = grid[i][j] + sums[i-1][j]
			} else {
				sums[i][j] = grid[i][j] + sums[i][j-1]
			}
		}
	}

	return sums[rows-1][columns-1]

}

func main() {
	input := [][]int{{1, 4, 2}, {1, 3, 2}, {2, 2, 1}}
	output := minPathSum(input)
	fmt.Println(output)
}
```

**输出**

```go
7
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力用示例覆盖所有概念。本教程适用于那些希望在 Golang 中获得专业知识并深入理解的人——[Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章适合你——[Golang 中的所有设计模式](https://golangbyexample.com/all-design-patterns-golang/)
