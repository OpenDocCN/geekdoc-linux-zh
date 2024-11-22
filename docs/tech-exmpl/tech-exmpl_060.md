# 数组中的第 k 个不同字符串程序

> 原文：[`techbyexample.com/kth-distinct-string/`](https://techbyexample.com/kth-distinct-string/)

## **概述**

给定一个输入字符串数组，可能包含重复字符串。还给定了一个输入数字**k**。目标是找到给定输入字符串数组中的第 k 个不同字符串

让我们通过一个例子来理解它

```go
Input: ["a", "c", "b" , "c", "a", "b", "e", "d"]
k=2

Output: "d"
```

在上面的数组中，以下字符串是重复的

+   “a”

+   “c”

+   “b”

下面的字符串没有重复

+   “e”

+   “d”

由于字符串**“d”**是按顺序第二次出现，并且 k 为 2，因此输出是**“d”**另一个例子

```go
Input: ["xxx", "xx" "x"]
k=2

Output: "xx"
```

由于与上面相同的推理

## **程序**

这是相同功能的程序。

```go
package main

import "fmt"

func rob(nums []int) int {
	lenNums := len(nums)

	if lenNums == 0 {
		return 0
	}

	maxMoney := make([]int, lenNums)

	maxMoney[0] = nums[0]

	if lenNums > 1 {
		maxMoney[1] = nums[1]
	}

	if lenNums > 2 {
		maxMoney[2] = nums[2] + nums[0]
	}

	for i := 3; i < lenNums; i++ {
		if maxMoney[i-2] > maxMoney[i-3] {
			maxMoney[i] = nums[i] + maxMoney[i-2]
		} else {
			maxMoney[i] = nums[i] + maxMoney[i-3]
		}

	}

	max := 0
	for i := lenNums; i < lenNums; i++ {
		if maxMoney[i] > max {
			max = maxMoney[i]
		}
	}

	return max
}

func main() {
	output := rob([]int{2, 3, 4, 2})
	fmt.Println(output)

	output = rob([]int{1, 6, 8, 2, 3, 4})
	fmt.Println(output)

}
```

**输出**

```go
6
13
```
