# 恢复二叉搜索树程序

> 原文：[`techbyexample.com/recover-binary-search-tree/`](https://techbyexample.com/recover-binary-search-tree/)

# **概述**

给定一个二叉搜索树的根节点。二叉搜索树中有两个节点被交换了。我们需要修复二叉树并恢复其原始结构

# **程序**

以下是相应的程序

```go
package main

import "fmt"

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func recoverTree(root *TreeNode) {
	var prev *TreeNode
	var first *TreeNode
	var mid *TreeNode
	var last *TreeNode

	recoverTreeUtil(root, &prev, &first, &mid, &last)

	if first != nil && last != nil {
		first.Val, last.Val = last.Val, first.Val
	} else if first != nil && mid != nil {
		first.Val, mid.Val = mid.Val, first.Val
	}
}

func recoverTreeUtil(root *TreeNode, prev, first, mid, last **TreeNode) {
	if root == nil {
		return
	}

	recoverTreeUtil(root.Left, prev, first, mid, last)

	if *prev == nil {
		*prev = root
	} else if *first == nil && (*prev).Val > root.Val {
		*first = *prev
		*mid = root
	} else if (*prev).Val > root.Val {
		*last = root
	}

	*prev = root

	recoverTreeUtil(root.Right, prev, first, mid, last)
}

func main() {
	root := TreeNode{Val: 2}
	root.Left = &TreeNode{Val: 3}
	root.Right = &TreeNode{Val: 1}

	recoverTree(&root)
	fmt.Println(root.Val)
	fmt.Println(root.Left.Val)
	fmt.Println(root.Right.Val)

}
```

**输出**

```go
2
1
3
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力通过实例覆盖所有概念。本教程适合那些希望深入学习并掌握 Golang 的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章适合你 – [Golang 中的所有设计模式](https://golangbyexample.com/all-design-patterns-golang/)

另外，查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
