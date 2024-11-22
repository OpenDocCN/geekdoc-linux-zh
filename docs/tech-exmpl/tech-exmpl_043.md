# 判断矩阵是否可以通过旋转获得

> 原文：[`techbyexample.com/program-matrix-rotation-target/`](https://techbyexample.com/program-matrix-rotation-target/)

# **概述**

给定两个 n*n 矩阵**源矩阵**和**目标矩阵**。我们需要确定是否可以通过进行任意次数的 90 度旋转将**源矩阵**转换为**目标矩阵**。

**示例 1**

```go
Input: source = [2,1],[1,2]], target = [[1,2],[2,1]]
Output: true
```

源矩阵可以旋转 90 度一次以获得目标矩阵。

**示例 2**

```go
Input:  source = [[1,2],[2,2]], target = [[2,1],[1,2]]
Output: false
```

即使我们将源矩阵旋转 90 度 3 次，也无法得到目标矩阵。

# **程序**

以下是相应的程序

```go
package main

import "fmt"

func findRotation(mat [][]int, target [][]int) bool {
	n, tmp := len(mat), make([]bool, 4)
	for i := range tmp {
		tmp[i] = true
	}
	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if mat[i][j] != target[i][j] {
				tmp[0] = false
			}
			if mat[i][j] != target[j][n-i-1] {
				tmp[1] = false
			}
			if mat[i][j] != target[n-i-1][n-j-1] {
				tmp[2] = false
			}
			if mat[i][j] != target[n-j-1][i] {
				tmp[3] = false
			}
		}
	}
	return tmp[0] || tmp[1] || tmp[2] || tmp[3]
}

func main() {
	output := findRotation([][]int{{2, 1}, {1, 2}}, [][]int{{1, 2}, {2, 1}})
	fmt.Println(output)

	output = findRotation([][]int{{1, 2}, {2, 2}}, [][]int{{2, 1}, {1, 2}})
	fmt.Println(output)

}
```

**输出**

```go
true
false
```

另外，查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
