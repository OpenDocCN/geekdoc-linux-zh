# 唯一路径程序

> 原文：[`techbyexample.com/unique-paths-program/`](https://techbyexample.com/unique-paths-program/)

## **概述**

有一个 m*n 的网格。机器人位于位置 (0,0)。机器人只能向右或向下移动。那么机器人到达右下角 (m-1, n-1) 的总路径数是多少？

示例

```go
Input: m=2 , n=2
Output: 2

Robot can reach the right down corner in two ways. 
1\. [0,0] -> [0,1]-> [1, 1]
2\. [0,0] -> [1,0]-> [1, 1]
```

这个程序还有另一种变体，其中网格中的某个位置可能包含障碍物。让我们先来看第一种变体，稍后再介绍第二种变体。

## **第一种变体**

我们将通过动态规划来解决这个问题

+   创建一个大小为 m*n 的路径矩阵

+   **paths[i][j]** 表示机器人到达 (i,j) 位置的路径数

+   **paths[0][0]** = 0

+   **paths[i][j]** = **paths[i-1][j]** + **paths[i][j-1]**

### **程序**

下面是相应的程序代码。

```go
package main

import "fmt"

func uniquePaths(m int, n int) int {
	paths := make([][]int, m)

	for i := 0; i < m; i++ {
		paths[i] = make([]int, n)
	}

	paths[0][0] = 1

	for i := 1; i < m; i++ {
		paths[i][0] = 1
	}

	for i := 1; i < n; i++ {
		paths[0][i] = 1
	}

	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			paths[i][j] = paths[i-1][j] + paths[i][j-1]
		}
	}

	return paths[m-1][n-1]
}

func main() {
	output := uniquePaths(3, 7)
	fmt.Println(output)
}
```

**输出**

```go
6
```

**第二种变体**我们也将通过动态规划来解决这个问题

+   创建一个大小为 m*n 的路径矩阵

+   **paths[i][j]** 表示机器人到达 (i,j) 位置的路径数

+   **paths[0][0]** = 0

+   如果**paths[i][j]**不是障碍物，则**paths[i][j]** = **paths[i-1][j]** + **paths[i][j-1]**

+   如果**paths[i][j]**是障碍物，则**paths[i][j]** = 0

### **程序**

```go
package main

import "fmt"

func uniquePathsWithObstacles(obstacleGrid [][]int) int {

	m := len(obstacleGrid)
	n := len(obstacleGrid[0])
	paths := make([][]int, len(obstacleGrid))

	for i := 0; i < m; i++ {
		paths[i] = make([]int, n)
	}

	if obstacleGrid[0][0] != 1 {
		paths[0][0] = 1
	}

	for i := 1; i < m; i++ {
		if obstacleGrid[i][0] == 1 {
			break
		} else {
			paths[i][0] = paths[i-1][0]
		}

	}

	for i := 1; i < n; i++ {
		if obstacleGrid[0][i] == 1 {
			break
		} else {
			paths[0][i] = paths[0][i-1]
		}

	}

	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			if obstacleGrid[i][j] != 1 {
				paths[i][j] = paths[i-1][j] + paths[i][j-1]
			}

		}
	}

	return paths[m-1][n-1]
}

func main() {
	output := uniquePathsWithObstacles([][]int{{0, 0, 0}, {0, 1, 0}, {0, 0, 0}})
	fmt.Println(output)
}
```

**输出**

```go
2
```