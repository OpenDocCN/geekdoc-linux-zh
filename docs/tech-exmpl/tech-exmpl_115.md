# 求二叉树高度的程序

> 原文：[`techbyexample.com/height-binary-tree/`](https://techbyexample.com/height-binary-tree/)

## **概述**

目标是获取二叉树的高度。例如，假设我们有如下二叉树

![](img/9a87d6b2a88fc41ddf4f755bc080acdf.png)

那么这个二叉树的高度是 3。

## **程序**

以下是相应的程序

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

func maxDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}
	if root.Left == nil && root.Right == nil {
		return 1
	}

	lHeight := maxDepth(root.Left)
	rHeight := maxDepth(root.Right)

	if lHeight >= rHeight {
		return lHeight + 1
	} else {
		return rHeight + 1
	}
}

func main() {
	root := TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Left.Left = &TreeNode{Val: 4}
	root.Right = &TreeNode{Val: 3}
	root.Right.Left = &TreeNode{Val: 5}
	root.Right.Right = &TreeNode{Val: 6}

	height := maxDepth(&root)
	fmt.Println(height)

}
```

**输出**

```go
3
```
