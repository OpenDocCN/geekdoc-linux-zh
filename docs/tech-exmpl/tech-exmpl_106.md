# 二叉树的后序遍历￼

> 原文：[`techbyexample.com/postorder-traversal-binary-tree/`](https://techbyexample.com/postorder-traversal-binary-tree/)

## **概述**

在二叉树的后序遍历中，我们遵循以下顺序

+   访问左子树

+   访问右子树

+   访问根节点

例如，假设我们有下面的二叉树

![](img/0a326e4e3810e8771142ce8b6cef7429.png)

然后后序遍历将是

```go
[4 2 5 6 3 1]
```

## **程序**

这是相应的程序

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

func postorderTraversal(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	left := postorderTraversal(root.Left)
	right := postorderTraversal(root.Right)

	output := make([]int, 0)

	output = append(output, left...)

	output = append(output, right...)

	output = append(output, root.Val)
	return output
}

func main() {
	root := TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Left.Left = &TreeNode{Val: 4}
	root.Right = &TreeNode{Val: 3}
	root.Right.Left = &TreeNode{Val: 5}
	root.Right.Right = &TreeNode{Val: 6}

	output := postorderTraversal(&root)
	fmt.Println(output)

}
```

**输出**

```go
[4 2 5 6 3 1]
```
