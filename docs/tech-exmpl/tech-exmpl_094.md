# 排序数组转高度平衡 BST 的程序

> 原文：[`techbyexample.com/program-sorted-array-bst/`](https://techbyexample.com/program-sorted-array-bst/)

## **概述**

给定一个排序数组，数组按升序排列。目标是将该排序数组转换为一个高度平衡的二叉搜索树。

平衡二叉搜索树（BST）是指左子树和右子树的高度差对于每个节点最大为 1 的二叉搜索树。

## **程序**

以下是该程序。

```go
package main

import "fmt"

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func sortedArrayToBST(nums []int) *TreeNode {
	length := len(nums)
	return sortedArrayToBSTUtil(nums, 0, length-1)
}

func sortedArrayToBSTUtil(nums []int, first int, last int) *TreeNode {

	if first > last {
		return nil
	}

	if first == last {
		return &TreeNode{
			Val: nums[first],
		}
	}

	mid := (first + last) / 2

	root := &TreeNode{
		Val: nums[mid],
	}

	root.Left = sortedArrayToBSTUtil(nums, first, mid-1)
	root.Right = sortedArrayToBSTUtil(nums, mid+1, last)
	return root
}

func traverseInorder(root *TreeNode) {
	if root == nil {
		return
	}

	traverseInorder(root.Left)
	fmt.Println(root.Val)
	traverseInorder(root.Right)
}

func main() {
	root := sortedArrayToBST([]int{1, 2, 3, 4, 5})
	traverseInorder(root)

}
```

**输出**

```go
1
2
3
4
5
```
