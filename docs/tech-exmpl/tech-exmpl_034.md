# 是否为二分图程序

> [`techbyexample.com/graph-bipartite-program/`](https://techbyexample.com/graph-bipartite-program/)

## **概述**

给定一个无向图。如果一个图的节点可以被分为两个子集，并且每条边连接一个子集中的节点与另一个子集中的某个节点，那么这个图是二分图。

图中包含 n 个节点，编号从**0**到**n-1**。输入是一个名为**graph**的矩阵，这是一个二维矩阵，其中 graph[i]包含第**i**个节点连接的节点。例如，如果

**graph[0] = [1,3]**

这意味着**节点 0**与**节点 1**和**节点 3**相连

**示例 1**

![](img/92669909d240e250e8a5663c7d9be398.png)

```go
Input: [[1,3],[0,2],[1,3],[0,2]]
Output: true
```

**示例 2**

![](img/9e4dffc2c4fa73c6afc6e4f4e355db2e.png)

```go
Input: [[1,4],[0,2],[1,3],[2,4],[0,3]
Output: false
```

这里的思路是使用深度优先搜索（DFS）。我们将尝试为每个节点分配红色或黑色。如果一个节点被涂上红色，那么它的邻居必须涂上黑色。

+   如果我们能以这种方式着色，那么图就是二分图

+   如果在着色过程中，我们发现两个通过边连接的节点有相同的颜色，那么该图不是二分图

让我们来看一下相应的程序

# **程序**

以下是相应的程序

```go
package main

import "fmt"

func isBipartite(graph [][]int) bool {
	nodeMap := make(map[int][]int)

	numNodes := len(graph)

	if numNodes == 1 {
		return true
	}
	for i := 0; i < numNodes; i++ {
		nodes := graph[i]
		for j := 0; j < len(nodes); j++ {
			nodeMap[i] = append(nodeMap[i], nodes[j])
		}
	}

	color := make(map[int]int)

	for i := 0; i < numNodes; i++ {
		if color[i] == 0 {
			color[i] = 1
			isBiPartite := visit(i, nodeMap, &color)
			if !isBiPartite {
				return false
			}
		}
	}

	return true

}

func visit(source int, nodeMap map[int][]int, color *map[int]int) bool {

	for _, neighbour := range nodeMap[source] {
		if (*color)[neighbour] == 0 {
			if (*color)[source] == 1 {
				(*color)[neighbour] = 2
			} else {
				(*color)[neighbour] = 1
			}
			isBipartite := visit(neighbour, nodeMap, color)
			if !isBipartite {
				return false
			}
		} else {
			if (*color)[source] == (*color)[neighbour] {
				return false
			}
		}
	}

	return true
}

func main() {
	output := isBipartite([][]int{{1, 3}, {0, 2}, {1, 3}, {0, 2}})
	fmt.Println(output)

	output = isBipartite([][]int{{1, 4}, {0, 2}, {1, 3}, {2, 4}, {0, 3}})
	fmt.Println(output)

}
```

**[输出:](http://Output:)**

```go
true
false
```

另外，请查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
