# 打印所有二叉树路径

> 原文：[`techbyexample.com/print-all-binary-tree-paths/`](https://techbyexample.com/print-all-binary-tree-paths/)

# **概述**

给定一棵树的根节点，目标是打印所有从根节点开始的树路径。

**示例**

![](img/9a87d6b2a88fc41ddf4f755bc080acdf.png)

**输出：**

```go
1->2->4
1->3->5
1->3->6
```

# **程序**

以下是相同的程序

```go
package main

import (
	"fmt"
	"strconv"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func binaryTreePaths(root *TreeNode) []string {
	output := make([]string, 0)

	binaryTreePathsUtil(root, "", &output)
	return output
}

func binaryTreePathsUtil(root *TreeNode, curr string, output *[]string) {
	if root == nil {
		return
	}

	valString := strconv.Itoa(root.Val)
	if curr == "" {
		curr = valString
	} else {
		curr = curr + "->" + valString
	}
	if root.Left == nil && root.Right == nil {
		*output = append(*output, curr)
		return
	}

	binaryTreePathsUtil(root.Left, curr, output)
	binaryTreePathsUtil(root.Right, curr, output)

}

func main() {
	root := TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Left.Left = &TreeNode{Val: 4}
	root.Right = &TreeNode{Val: 3}
	root.Right.Left = &TreeNode{Val: 5}
	root.Right.Right = &TreeNode{Val: 6}

	output := binaryTreePaths(&root)
	fmt.Println(output)
}
```

**输出：**

```go
[1->2->4 1->3->5 1->3->6]
```

同时，查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)
