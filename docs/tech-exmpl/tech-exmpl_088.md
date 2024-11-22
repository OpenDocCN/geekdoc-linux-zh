# 从链表中删除倒数第 n 个节点

> 原文：[`techbyexample.com/remove-nth-node-end-linked-list/`](https://techbyexample.com/remove-nth-node-end-linked-list/)

## **概述**

目标是从链表的末尾删除倒数第 n 个节点。如果链表的大小为**x**，那么倒数第 n 个节点的位置是(x-n+1)

示例

```go
Input: 1->2->3->4->5
Node to be deleted from the end: 2
Output: 1->2->3->5
```

## **程序**

这是相应的程序。

```go
package main

import "fmt"

func main() {
	first := initList()
	first.AddFront(5)
	first.AddFront(4)
	first.AddFront(3)
	first.AddFront(2)
	first.AddFront(1)

	first.Head.Traverse()
	removeNthFromEnd(first.Head, 2)
	fmt.Println("")
	first.Head.Traverse()

}

func initList() *SingleList {
	return &SingleList{}
}

type ListNode struct {
	Val  int
	Next *ListNode
}

func (l *ListNode) Traverse() {
	for l != nil {
		fmt.Println(l.Val)
		l = l.Next
	}
}

type SingleList struct {
	Len  int
	Head *ListNode
}

func (s *SingleList) AddFront(num int) {
	ele := &ListNode{
		Val: num,
	}
	if s.Head == nil {
		s.Head = ele
	} else {
		ele.Next = s.Head
		s.Head = ele
	}
	s.Len++
}

func removeNthFromEnd(head *ListNode, n int) *ListNode {
	size := lengthOfList(head)

	numberFront := size - n + 1

	if numberFront < 1 {
		return head
	}

	if numberFront == 1 {
		return head.Next
	}

	if size == 1 {
		return nil
	}

	//Get to the numberFront-1 node
	curr := head
	for i := 0; i < numberFront-2; i++ {
		curr = curr.Next
	}

	prev := curr

	nodeToDelete := curr.Next

	if nodeToDelete != nil {
		nodeToDeleteNext := nodeToDelete.Next
		prev.Next = nodeToDeleteNext
	} else {
		prev.Next = nil
	}

	return head
}

func lengthOfList(head *ListNode) int {
	size := 0
	for head != nil {
		size = size + 1
		head = head.Next
	}
	return size
}
```

**输出**

```go
1
2
3
4
5

1
2
3
5
```
