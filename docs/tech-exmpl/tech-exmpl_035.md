# 二叉树最大路径和程序

> 原文：[`techbyexample.com/binary-tree-maximum-path-sum/`](https://techbyexample.com/binary-tree-maximum-path-sum/)

# **概览**

给定一个二叉树，目标是找到该二叉树中的最大路径和。二叉树中的路径是指一系列彼此相连的节点，每个节点在最大路径和中只能出现一次。

**示例 1**

![](img/9a87d6b2a88fc41ddf4f755bc080acdf.png)

```go
Output: 16
Maximum Sum Path is: 4->2->1->3->6
```

**示例 2**

![](img/6c1876d0d5a5830a4bbf871b9fa3cdc9.png)

```go
Output: 14
Maximum Sum Path is: 5->3->6
```

该方法的思路是在每个节点跟踪以下四个值。

+   a = root.Val

+   b = root.Val + leftSubTreeMaxSum

+   c = root.Val + rightSubTreeMaxSum

+   d = root.Val + leftSubTreeMaxSum + rightSubTreeMaxSum

然后

+   给定节点的最大路径和是**(a,b,c,d)**中的最大值。

+   递归调用中的返回值将是**(a,b,c)**中的最大值。为什么？这是因为只有 a、b 或 c 的路径才是父节点可以考虑的路径，d 不能考虑，因为它成为了无效路径。通过上面示例二的二叉树来理解这个问题。路径**5->3->6**不能包括父节点**-5**，因为它成为了无效路径。

# **程序**

以下是相应的程序代码：

```go
package main

import (
	"fmt"
	"math"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func maxPathSum(root *TreeNode) int {
	res := math.MinInt64

	maxPathSumUtil(root, &res)
	return res
}

func maxPathSumUtil(root *TreeNode, res *int) int {
	if root == nil {
		return 0
	}

	l := maxPathSumUtil(root.Left, res)
	r := maxPathSumUtil(root.Right, res)

	a := root.Val
	b := root.Val + l
	c := root.Val + r
	d := root.Val + l + r

	maxReturnSum := maxOfThree(a, b, c)

	maxSumPath := maxOfTwo(maxReturnSum, d)
	if maxSumPath > *res {
		*res = maxSumPath
	}

	return maxReturnSum
}

func maxOfThree(a, b, c int) int {
	if a > b && a > c {
		return a
	}

	if b > c {
		return b
	}

	return c
}

func maxOfTwo(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	root := &TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Left.Left = &TreeNode{Val: 4}
	root.Right = &TreeNode{Val: 3}
	root.Right.Left = &TreeNode{Val: 5}
	root.Right.Right = &TreeNode{Val: 6}

	output := maxPathSum(root)
	fmt.Println(output)

	root = &TreeNode{Val: -10}
	root.Left = &TreeNode{Val: 2}
	root.Left.Left = &TreeNode{Val: 4}
	root.Right = &TreeNode{Val: 3}
	root.Right.Left = &TreeNode{Val: 5}
	root.Right.Right = &TreeNode{Val: 6}
	output = maxPathSum(root)
	fmt.Println(output)

}
```

**输出：**

```go
16
14
```

另外，查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
