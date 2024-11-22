# 设计一个内存缓存

> 原文：[`techbyexample.com/design-in-memory-cache/`](https://techbyexample.com/design-in-memory-cache/)

目录

+   概述

+   设置(key int, value int)")

+   获取(key int)")

+   低级设计

+   UML 图

+   程序

+   结论

## **概述**

目标是设计一个内存缓存。以下是要求

+   它应该支持**设置**和**获取**操作

+   **设置**和**获取**操作的时间复杂度为 O(1)

+   假设缓存的最大容量为 3。当缓存已满并且有一个新键需要插入时，必须删除缓存中的一个现有条目

+   删除应该基于驱逐算法——FIFO 或 LRU

+   假设缓存中的键和值都是字符串类型

+   它应该支持的驱逐算法是**先进先出(FIFO)**和**最近最少使用(LRU)**

+   驱逐算法应该是可插拔的。这意味着你应该能够在运行时更改驱逐算法。

以下是我们设计中的高层内容

+   我们将拥有一个**缓存**类，作为与客户端交互的接口

+   **缓存**类将使用**映射**和**双向链表**的组合来存储所有内容。使用映射和双向链表是为了确保即使在驱逐情况下，获取和设置操作的时间复杂度依然是 O(1)。我们将在本教程的后续部分详细介绍如何实现。

+   **映射**将具有字符串类型的键，以及指向**双向链表**节点的指针类型值

+   每个**双向链表**的节点将包含键和值。每个节点还将有一个指向双向链表中前一个节点的指针，以及一个指向下一个节点的指针

+   将会有一个**驱逐算法**接口。将有**LRU**和**FIFO**类来实现这个**驱逐算法**接口

+   缓存类还将嵌入一个**驱逐算法**接口实例。

**驱逐算法**接口的存在是为了将我们的**缓存**类与算法解耦，以便我们可以在运行时更改算法。而且，当添加新算法时，**缓存**类不应发生变化。这就是**策略设计模式**的应用场景。策略模式建议创建一个算法家族，每个算法都有自己的类。这些类遵循相同的接口，使得算法在这个家族中是可互换的。假设通用接口名称是**驱逐算法**，那么**FIFO**和**LRU**类将实现这个接口。

现在我们的主 **Cache** 类将嵌入 **evictionAlgo** 接口。我们的 Cache 类不会在自己内部实现所有类型的驱逐算法，而是将所有的工作委托给 **evictionAlgo** 接口。由于 evictionAlgo 是一个接口，我们可以在运行时改变算法，使其成为 LRU 或 FIFO，而无需修改 Cache 类。

此外，我们将使用工厂设计模式来创建每个算法的实例，即 FIFO、LRU。

让我们看看 Get 和 Set 如何在 O(1) 时间内工作。

## **Set(key int, value int)**

对于任何 set 操作，它将首先创建一个带有提供的 key 和 value 的双向链表节点。然后，在 map 中创建一个条目，key 为输入的 key，value 为该节点的地址。节点创建后，有两种情况：

+   **缓存未满** – 在这种情况下，它将控制权交给当前的 evictionAlgorithm 接口。evictionAlgorithm 接口将根据当前的驱逐算法，将该节点插入到双向链表的末尾或前面。这里的每个操作都是 O(1)。

+   **缓存已满** – 在这种情况下，它将控制权交给当前的 evictionAlgorithm 接口，根据当前的驱逐算法驱逐其中一个节点。驱逐后，它会插入新节点。这里的每个操作都是 O(1)。

## **Get(key int)**

对于任何 Get 操作，它将首先检查 map 中是否存在给定的 key。如果存在，它将获取该 key 在 map 中指向的节点的地址。然后，它会从节点中获取值。接着，它会将控制权交给当前的 evictionAlgorithm 接口。evictionAlgorithm 接口将根据当前的驱逐算法，重新排列当前节点在双向链表中的位置，可能是末尾或前面。这里的每个操作都是 O(1)。

## **低级设计**

以下是用 Go 编程语言表达的低级设计。稍后我们将看到一个 UML 图以及一个工作示例。

**Cache 类**

```go
type cache struct {
	dll          *doublyLinkedList
	storage      map[string]string
	evictionAlgo evictionAlgo
	capacity     int
	maxCapacity  int
}

func (c *cache) setEvictionAlgo(e evictionAlgo)

func (c *cache) add(key, value string)

func (c *cache) get(key string)

func (c *cache) evict()
```

**双向链表**

```go
type node struct {
    data string
    prev *node
    next *node
}

type doublyLinkedList struct {
    len  int
    tail *node
    head *node
}

func initDoublyList() *doublyLinkedList {
    return &doublyLinkedList{}
}

func (d *doublyLinkedList) AddFrontNodeDLL(data string) 

func (d *doublyLinkedList) AddEndNodeDLL(data string) 

func (d *doublyLinkedList) MoveToFront(node *node) error 

func (d *doublyLinkedList) MoveToBack(node *node) error

func (d *doublyLinkedList) Size() int
```

**驱逐算法接口**

```go
type evictionAlgo interface {
	evict(c *cache)
}

func createEvictioAlgo(algoType string) evictionAlgo
```

**FIFO 算法** – 它实现了驱逐算法接口。

```go
type fifo struct {
}

func (l *fifo) evict(c *cache)
```

**LRU 算法** – 它实现了驱逐算法接口

```go
type lru struct {
}

func (l *lru) evict(c *cache)
```

## **UML 图**

![](img/93ba75ea015d9c055a7f090acda77d83.png)

## **程序**

这是用 Go 编程语言编写的完整工作代码，如果有兴趣的人可以查看。

**doublylinklist.go**

```go
package main

import "fmt"

type node struct {
	key   string
	value string
	prev  *node
	next  *node
}

type doublyLinkedList struct {
	len  int
	tail *node
	head *node
}

func initDoublyList() *doublyLinkedList {
	return &doublyLinkedList{}
}

func (d *doublyLinkedList) AddToFront(key, value string) {
	newNode := &node{
		key:   key,
		value: value,
	}
	if d.head == nil {
		d.head = newNode
		d.tail = newNode
	} else {
		newNode.next = d.head
		d.head.prev = newNode
		d.head = newNode
	}
	d.len++
	return
}

func (d *doublyLinkedList) RemoveFromFront() {
	if d.head == nil {
		return
	} else if d.head == d.tail {
		d.head = nil
		d.tail = nil
	} else {
		d.head = d.head.next
	}
	d.len--
}

func (d *doublyLinkedList) AddToEnd(node *node) {
	newNode := node
	if d.head == nil {
		d.head = newNode
		d.tail = newNode
	} else {
		currentNode := d.head
		for currentNode.next != nil {
			currentNode = currentNode.next
		}
		newNode.prev = currentNode
		currentNode.next = newNode
		d.tail = newNode
	}
	d.len++
}
func (d *doublyLinkedList) Front() *node {
	return d.head
}

func (d *doublyLinkedList) MoveNodeToEnd(node *node) {
	prev := node.prev
	next := node.next

	if prev != nil {
		prev.next = next
	}

	if next != nil {
		next.prev = prev
	}
	if d.tail == node {
		d.tail = prev
	}
	if d.head == node {
		d.head = next
	}
	node.next = nil
	node.prev = nil
	d.len--
	d.AddToEnd(node)
}

func (d *doublyLinkedList) TraverseForward() error {
	if d.head == nil {
		return fmt.Errorf("TraverseError: List is empty")
	}
	temp := d.head
	for temp != nil {
		fmt.Printf("key = %v, value = %v, prev = %v, next = %v\n", temp.key, temp.value, temp.prev, temp.next)
		temp = temp.next
	}
	fmt.Println()
	return nil
}

func (d *doublyLinkedList) Size() int {
	return d.len
}
```

**evictionAlgorithm.go**

```go
package main

type evictionAlgo interface {
	evict(c *Cache) string
	get(node *node, c *Cache)
	set(node *node, c *Cache)
	set_overwrite(node *node, value string, c *Cache)
}

func createEvictioAlgo(algoType string) evictionAlgo {
	if algoType == "fifo" {
		return &fifo{}
	} else if algoType == "lru" {
		return &lru{}
	}

	return nil
}
```

**lru.go**

```go
package main

import "fmt"

type lru struct {
}

func (l *lru) evict(c *Cache) string {
	key := c.doublyLinkedList.Front().key
	fmt.Printf("Evicting by lru strtegy. Evicted Node Key: %s: ", key)
	c.doublyLinkedList.RemoveFromFront()
	return key
}

func (l *lru) get(node *node, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to get operation")
	c.doublyLinkedList.MoveNodeToEnd(node)
}

func (l *lru) set(node *node, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to set operation")
	c.doublyLinkedList.AddToEnd(node)
}

func (l *lru) set_overwrite(node *node, value string, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to set_overwrite operation")
	node.value = value
	c.doublyLinkedList.MoveNodeToEnd(node)
}
```

**fifo.go**

```go
package main

import "fmt"

type fifo struct {
}

func (l *fifo) evict(c *Cache) string {
	fmt.Println("Evicting by fifo strtegy")
	key := c.doublyLinkedList.Front().key
	c.doublyLinkedList.RemoveFromFront()
	return key
}

func (l *fifo) get(node *node, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to get operation")
}

func (l *fifo) set(node *node, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to set operation")
	c.doublyLinkedList.AddToEnd(node)
}

func (l *fifo) set_overwrite(node *node, value string, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to set_overwrite operation")
}
```

**cache.go**

```go
package main

import "fmt"

type Cache struct {
	doublyLinkedList *doublyLinkedList
	storage          map[string]*node
	evictionAlgo     evictionAlgo
	capacity         int
	maxCapacity      int
}

func initCache(evictionAlgo evictionAlgo, maxCapacity int) Cache {
	storage := make(map[string]*node)
	return Cache{
		doublyLinkedList: &doublyLinkedList{},
		storage:          storage,
		evictionAlgo:     evictionAlgo,
		capacity:         0,
		maxCapacity:      maxCapacity,
	}
}

func (this *Cache) setEvictionAlgo(e evictionAlgo) {
	this.evictionAlgo = e
}

func (this *Cache) set(key, value string) {
	node_ptr, ok := this.storage[key]
	if ok {
		this.evictionAlgo.set_overwrite(node_ptr, value, this)
		return
	}
	if this.capacity == this.maxCapacity {
		evictedKey := this.evict()
		delete(this.storage, evictedKey)
	}
	node := &node{key: key, value: value}
	this.storage[key] = node
	this.evictionAlgo.set(node, this)
	this.capacity++
}

func (this *Cache) get(key string) string {
	node_ptr, ok := this.storage[key]
	if ok {
		this.evictionAlgo.get(node_ptr, this)
		return (*node_ptr).value
	}
	return ""
}

func (this *Cache) evict() string {
	key := this.evictionAlgo.evict(this)
	this.capacity--
	return key
}

func (this *Cache) print() {
	for k, v := range this.storage {
		fmt.Printf("key :%s value: %s\n", k, (*v).value)
	}
	this.doublyLinkedList.TraverseForward()
}
```

**main.go**

```go
package main

import "fmt"

func main() {
	lru := createEvictioAlgo("lru")
	cache := initCache(lru, 3)
	cache.set("a", "1")
	cache.print()

	cache.set("b", "2")
	cache.print()

	cache.set("c", "3")
	cache.print()

	value := cache.get("a")
	fmt.Printf("key: a, value: %s\n", value)
	cache.print()

	cache.set("d", "4")
	cache.print()

	cache.set("e", "5")
	cache.print()
}
```

**输出**

```go
Shuffling doubly linked list due to set operation
key :a value: 1
key = a, value = 1, prev = <nil>, next = <nil>

Shuffling doubly linked list due to set operation
key :a value: 1
key :b value: 2
key = a, value = 1, prev = <nil>, next = &{b 2 0xc00007e1e0 <nil>}
key = b, value = 2, prev = &{a 1 <nil> 0xc00007e210}, next = <nil>

Shuffling doubly linked list due to set operation
key :a value: 1
key :b value: 2
key :c value: 3
key = a, value = 1, prev = <nil>, next = &{b 2 0xc00007e1e0 0xc00007e2a0}
key = b, value = 2, prev = &{a 1 <nil> 0xc00007e210}, next = &{c 3 0xc00007e210 <nil>}
key = c, value = 3, prev = &{b 2 0xc00007e1e0 0xc00007e2a0}, next = <nil>

Shuffling doubly linked list due to get operation
key: a, value: 1
key :a value: 1
key :b value: 2
key :c value: 3
key = b, value = 2, prev = <nil>, next = &{c 3 0xc00007e210 0xc00007e1e0}
key = c, value = 3, prev = &{b 2 <nil> 0xc00007e2a0}, next = &{a 1 0xc00007e2a0 <nil>}
key = a, value = 1, prev = &{c 3 0xc00007e210 0xc00007e1e0}, next = <nil>

Evicting by lru strtegy. Evicted Node Key: %s:  b
Shuffling doubly linked list due to set operation
key :d value: 4
key :c value: 3
key :a value: 1
key = c, value = 3, prev = &{b 2 <nil> 0xc00007e2a0}, next = &{a 1 0xc00007e2a0 0xc00007e450}
key = a, value = 1, prev = &{c 3 0xc00007e210 0xc00007e1e0}, next = &{d 4 0xc00007e1e0 <nil>}
key = d, value = 4, prev = &{a 1 0xc00007e2a0 0xc00007e450}, next = <nil>

Evicting by lru strtegy. Evicted Node Key: %s:  c
Shuffling doubly linked list due to set operation
key :a value: 1
key :d value: 4
key :e value: 5
key = a, value = 1, prev = &{c 3 0xc00007e210 0xc00007e1e0}, next = &{d 4 0xc00007e1e0 0xc00007e570}
key = d, value = 4, prev = &{a 1 0xc00007e2a0 0xc00007e450}, next = &{e 5 0xc00007e450 <nil>}
key = e, value = 5, prev = &{d 4 0xc00007e1e0 0xc00007e570}, next = <nil>
```

## **结论**

这就是设计内存缓存的全部内容。希望你喜欢这篇文章。请在评论中分享反馈。

+   [cache](https://techbyexample.com/tag/cache/)*   [fifo](https://techbyexample.com/tag/fifo/)*   [lru](https://techbyexample.com/tag/lru/)
