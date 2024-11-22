# 相同的二叉树程序￼

> 原文：[`techbyexample.com/same-binary-tree-program%EF%BF%BC/`](https://techbyexample.com/same-binary-tree-program%EF%BF%BC/)

## **概述**

目标是判断给定的两个二叉树是否相同。以下是相同的树示例

**树 1**

![](img/0a326e4e3810e8771142ce8b6cef7429.png)

**树 2**

![](img/0a326e4e3810e8771142ce8b6cef7429.png)

## **程序**

这是相同程序的代码。

```go
package main

import (
	"fmt"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func isSameTree(p *TreeNode, q *TreeNode) bool {
	if p == nil && q == nil {
		return true
	}

	if p == nil || q == nil {
		return false
	}

	if p.Val != q.Val {
		return false
	}

	return isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}

func main() {
	root1 := TreeNode{Val: 1}
	root1.Left = &TreeNode{Val: 2}
	root1.Left.Left = &TreeNode{Val: 4}
	root1.Right = &TreeNode{Val: 3}
	root1.Right.Left = &TreeNode{Val: 5}
	root1.Right.Right = &TreeNode{Val: 6}

	root2 := TreeNode{Val: 1}
	root2.Left = &TreeNode{Val: 2}
	root2.Left.Left = &TreeNode{Val: 4}
	root2.Right = &TreeNode{Val: 3}
	root2.Right.Left = &TreeNode{Val: 5}
	root2.Right.Right = &TreeNode{Val: 6}

	output := isSameTree(&root1, &root2)
	fmt.Println(output)

	root1 = TreeNode{Val: 1}
	root1.Left = &TreeNode{Val: 2}

	root2 = TreeNode{Val: 1}
	root2.Left = &TreeNode{Val: 3}

	output = isSameTree(&root1, &root2)
	fmt.Println(output)
}
```

**输出**

```go
true
false
```
