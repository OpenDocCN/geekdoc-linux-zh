# 程序：旋转链表

> 原文：[`techbyexample.com/program-to-rotate-linked-list/`](https://techbyexample.com/program-to-rotate-linked-list/)

## **概述**

目标是将给定的链表按照指定的位数朝正确的方向旋转。

示例

```go
Input: 1->2->3->4->5
k: 2

Output: 4->5->1->2->3
```

## **程序**

下面是相应的程序。

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

	head := rotateRight(first.Head, 2)
	fmt.Println("")
	head.Traverse()
	fmt.Println("")

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
		fmt.Print(l.Val)
		l = l.Next
	}
}

type SingleList struct {
	Len  int
	Head *ListNode
}

func (s *SingleList) Reverse() {

	curr := s.Head
	var prev *ListNode
	var next *ListNode

	for curr != nil {
		next = curr.Next
		curr.Next = prev
		prev = curr
		curr = next
	}
	s.Head = prev
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

func rotateRight(head *ListNode, k int) *ListNode {

	if head == nil {
		return nil
	}

	if k == 0 {
		return head
	}

	lenList := 0

	curr := head
	var last *ListNode

	for curr != nil {
		lenList++
		last = curr
		curr = curr.Next
	}

	k = k % lenList
	if k == 0 {
		return head
	}

	curr = head

	newHeadIndex := lenList - k

	var prev *ListNode

	for i := 0; i < newHeadIndex; i++ {
		prev = curr
		curr = curr.Next
	}

	newHeadNode := prev.Next

	prev.Next = nil

	last.Next = head
	return newHeadNode
}
```

**输出**

```go
12345
45123
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力通过示例覆盖所有概念。本教程适合那些希望掌握并深入理解 Golang 的人——[Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是，那么这篇文章适合你——[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)
