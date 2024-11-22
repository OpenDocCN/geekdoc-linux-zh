# 数组中的多数元素

> 原文：[`techbyexample.com/majority-element-array/`](https://techbyexample.com/majority-element-array/)

# **概述**

目标是找出给定数组中的多数元素。多数元素是指在给定数组中出现次数超过 n/2 的元素，其中 n 是数组的长度

**示例 1**

```go
Input: [2, 1, 2, 2, 3]
Output: 2
```

**示例 2**

```go
Input: [1]
Output: 1
```

# **程序**

以下是相应的程序

```go
package main

import "fmt"

func majorityElement(nums []int) int {
	lenNums := len(nums)

	if lenNums == 1 {
		return nums[0]
	}

	numsMap := make(map[int]int)

	for i := 0; i < lenNums; i++ {
		_, ok := numsMap[nums[i]]
		if ok {
			numsMap[nums[i]] = numsMap[nums[i]] + 1
			if numsMap[nums[i]] > lenNums/2 {
				return nums[i]
			}
		} else {
			numsMap[nums[i]] = 1
		}
	}

	return 0

}

func main() {
	output := majorityElement([]int{2, 1, 2, 2, 3})
	fmt.Println(output)

	output = majorityElement([]int{1})
	fmt.Println(output)
}
```

**输出：**

```go
2
1
```

同时，查看我们的系统设计教程系列：[系统设计教程系列](https://techbyexample.com/system-design-questions/)
