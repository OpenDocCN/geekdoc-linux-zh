# 用于检查给定链表是否有环的程序

> 原文：[`techbyexample.com/program-linked-list-cycle/`](https://techbyexample.com/program-linked-list-cycle/)

## **概述**

目标是检查给定的链表是否有环。如果链表中的最后一个节点指向链表中前面的某个节点，那么链表中就存在环。

示例

![](img/31d74a94feb5baa6a07151b35d2b3abb.png)

上面的链表有环。下面是我们可以遵循的方法。

+   有两个指针，一个是慢指针，另一个是快指针。两者最初都指向头节点。

+   现在将慢指针移动一个节点，快指针移动两个节点。

```go
slow := slow.Next
fast := fast.Next.Next
```

+   如果慢指针和快指针在任何时刻相同，则链表中存在环。

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func main() {
	first := initList()
	ele4 := first.AddFront(4)
	first.AddFront(3)
	ele2 := first.AddFront(2)
	first.AddFront(1)

	//Create cycle
	ele4.Next = ele2

	output := hasCycle(first.Head)
	fmt.Println(output)

}

type ListNode struct {
	Val  int
	Next *ListNode
}

type SingleList struct {
	Len  int
	Head *ListNode
}

func (s *SingleList) AddFront(num int) *ListNode {
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
	return ele
}

func initList() *SingleList {
	return &SingleList{}
}
func hasCycle(head *ListNode) bool {

	if head == nil || head.Next == nil {
		return false
	}

	hasCycle := false
	slow := head
	fast := head

	for slow != nil && fast != nil && fast.Next != nil {
		slow = slow.Next
		fast = fast.Next.Next

		if slow == fast {
			hasCycle = true
			break
		}
	}

	return hasCycle

}
```

**输出**

```go
true
```
