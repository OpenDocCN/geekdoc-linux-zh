# 分区链表的程序

> 原文：[`techbyexample.com/program-partition-linked-list/`](https://techbyexample.com/program-partition-linked-list/)

## **概述**

给定一个链表，并且给定一个目标值。将链表进行分区，使得所有小于目标值的节点排在大于目标值的节点之前。

示例

```go
Input: 4->5->3->1
Output: 2
Target: 1->4->5->3
```

原始顺序应保持不变

## **程序**

以下是相应的程序。

```go
package main

import "fmt"

func main() {
	first := initList()
	first.AddFront(1)
	first.AddFront(2)
	first.AddFront(3)
	first.AddFront(4)

	first.Head.Traverse()
	newHead := partition(first.Head, 2)
	fmt.Println("")
	newHead.Traverse()

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

func partition(head *ListNode, x int) *ListNode {
	if head == nil {
		return nil
	}

	curr := head

	var prev *ListNode

	for curr != nil {
		if curr.Val >= x {
			break
		}
		prev = curr
		curr = curr.Next
	}

	if curr == nil {
		return head
	}

	firstLargeValueNode := curr

	prev2 := firstLargeValueNode
	for curr != nil {
		if curr.Val < x {
			prev2.Next = curr.Next
			if prev != nil {
				prev.Next = curr
				prev = prev.Next
				prev.Next = firstLargeValueNode
			} else {
				if head == firstLargeValueNode {
					head = curr
				}
				curr.Next = firstLargeValueNode
				prev = curr
			}
		}
		prev2 = curr
		curr = curr.Next
	}

	return head
}
```

**输出**

```go
4
5
3
1

1
4
5
3
```
