# 计算未被守卫的单元程序

> 原文：[`techbyexample.com/count-unguarded-cells-program/`](https://techbyexample.com/count-unguarded-cells-program/)

# **概述**

给定两个整数 m 和 n，表示一个 m*n 的网格。此外，还给定了两个二维整数数组

+   guards，其中 guards[i] = [rowi, columni]。它表示第 i 个守卫的位置

+   walls，其中 guards[j] = [rowi, columni]。它表示第 j 个墙的位置

一个守卫可以看到所有方向

+   北

+   南

+   东

+   西

除非被墙阻挡。所有守卫能够看到的单元都会被视为已守卫。目标是找出未被守卫的单元数量

这是相同题目的 Leetcode 链接 – https://leetcode.com/problems/count-unguarded-cells-in-the-grid/

# **程序**

以下是相同程序的代码

```go
package main

import "fmt"

func countUnguarded(m int, n int, guards [][]int, walls [][]int) int {

	occupancy := make([][]int, m)

	for i := 0; i < m; i++ {
		occupancy[i] = make([]int, n)
	}

	for _, val := range guards {
		i := val[0]
		j := val[1]
		occupancy[i][j] = 2
	}

	for _, val := range walls {
		i := val[0]
		j := val[1]
		occupancy[i][j] = -1
	}

	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if occupancy[i][j] == 2 {
				countUtil(i, j, m, n, &occupancy)
			}

		}
	}

	count := 0
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if occupancy[i][j] == 0 {
				count += 1
			}

		}
	}

	return count
}

func countUtil(x, y, m, n int, occupancy *([][]int)) {

	for i := x + 1; i < m; i++ {
		if (*occupancy)[i][y] == 0 {
			(*occupancy)[i][y] = 1
		}

		if (*occupancy)[i][y] == -1 || (*occupancy)[i][y] == 2 {
			break
		}
	}

	for i := x - 1; i >= 0; i-- {
		if (*occupancy)[i][y] == 0 {
			(*occupancy)[i][y] = 1
		}

		if (*occupancy)[i][y] == -1 || (*occupancy)[i][y] == 2 {
			break
		}
	}

	for i := y + 1; i < n; i++ {
		if (*occupancy)[x][i] == 0 {
			(*occupancy)[x][i] = 1
		}

		if (*occupancy)[x][i] == -1 || (*occupancy)[x][i] == 2 {
			break
		}
	}

	for i := y - 1; i >= 0; i-- {

		if (*occupancy)[x][i] == 0 {
			(*occupancy)[x][i] = 1
		}

		if (*occupancy)[x][i] == -1 || (*occupancy)[x][i] == 2 {
			break
		}
	}

}

func main() {
	output := countUnguarded(4, 6, [][]int{{0, 0}, {1, 1}, {2, 3}}, [][]int{{0, 1}, {2, 2}, {1, 4}})
	fmt.Println(output)

	output = countUnguarded(3, 3, [][]int{{1, 1}}, [][]int{{0, 1}, {1, 0}, {2, 1}, {1, 2}})
	fmt.Println(output)
}
```

**输出**

```go
7
4
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详细，我们已经尽力通过实例涵盖所有概念。本教程适合那些希望深入理解 Golang 并获得专业知识的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果你感兴趣，那么这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

另外，请查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
