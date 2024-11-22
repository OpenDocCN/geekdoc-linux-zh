# 如果某行或某列为零，则将矩阵置为零

> 原文：[`techbyexample.com/set-matrix-zero/`](https://techbyexample.com/set-matrix-zero/)

## **概述**

给定一个 m*n 的矩阵。如果某个元素为零，则将其所在的行和列设置为零

示例

**输入：**

```go
1, 1, 1 
0, 1, 1 
1, 1, 1
```

**输出：**

```go
0, 1, 1 
0, 0, 0 
0, 1, 1
```

我们将通过使用两个额外的数组**rowSet**和**columnSet**，它们的长度分别为**m**和**n**，来解决这个问题。我们将遍历整个矩阵并

```go
if matrix[i][j] == 0 then
   rowSet[i] = 1
   columnSet[j] = 1
```

然后，如果**rowSet[i]**等于 1 或**columnSet[j]**等于 1，我们可以将**matrix[i][j]**设置为零

## **程序**

这是相应的程序。

```go
package main

import "fmt"

func setZeroes(matrix [][]int) [][]int {
	numRows := len(matrix)

	numColumns := len(matrix[0])

	rowSet := make([]int, numRows)
	columnSet := make([]int, numColumns)

	for i := 0; i < numRows; i++ {
		for j := 0; j < numColumns; j++ {
			if matrix[i][j] == 0 {
				rowSet[i] = 1
				columnSet[j] = 1
			}
		}
	}

	for i := 0; i < numRows; i++ {
		for j := 0; j < numColumns; j++ {
			if rowSet[i] == 1 || columnSet[j] == 1 {

				matrix[i][j] = 0

			}
		}
	}

	return matrix

}

func main() {
	matrix := [][]int{{1, 1, 1}, {0, 1, 1}, {1, 1, 1}}
	output := setZeroes(matrix)
	fmt.Println(output)
}
```

**输出**

```go
[[0 1 1] [0 0 0] [0 1 1]]
```
