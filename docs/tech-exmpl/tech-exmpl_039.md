# 帕斯卡三角形程序

> 原文：[`techbyexample.com/program-for-pascals-triangle/`](https://techbyexample.com/program-for-pascals-triangle/)

## **概述**

目标是打印帕斯卡三角形的 n 行，n 的值作为输入传递给程序

**示例 1**

```go
Input: numRows = 4
Output: [[1],[1,1],[1,2,1],[1,3,3,1]]
```

**示例 2**

```go
Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

请参阅此链接了解更多关于帕斯卡三角形的信息：[`en.wikipedia.org/wiki/Pascal%27s_triangle`](https://en.wikipedia.org/wiki/Pascal%27s_triangle)

这里的思路是使用动态规划。

# **程序**

以下是相应的程序

```go
package main

import "fmt"

func generate(numRows int) [][]int {
	firstRow := []int{1}

	if numRows == 1 {
		return [][]int{firstRow}
	}

	secondRow := []int{1, 1}

	if numRows == 2 {
		return [][]int{firstRow, secondRow}
	}

	output := [][]int{firstRow, secondRow}

	for i := 2; i < numRows; i++ {
		temp := make([]int, i+1)

		lastRow := output[i-1]

		temp[0] = 1
		temp[i] = 1

		for j := 1; j < i; j++ {
			temp[j] = lastRow[j-1] + lastRow[j]
		}

		output = append(output, temp)

	}

	return output

}

func main() {
	output := generate(4)
	fmt.Println(output)

	output = generate(5)
	fmt.Println(output)
}
```

**输出：**

```go
[1] [1 1] [1 2 1] [1 3 3 1]]
[[1] [1 1] [1 2 1] [1 3 3 1] [1 4 6 4 1]]
```

另外，请查看我们的系统设计教程系列：[系统设计教程系列](https://techbyexample.com/system-design-questions/)
