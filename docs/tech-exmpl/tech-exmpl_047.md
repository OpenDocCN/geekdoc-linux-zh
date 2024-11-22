# 在数组中找到只出现一次的数字

> 原文：[`techbyexample.com/number-appear-array/`](https://techbyexample.com/number-appear-array/)

# **概述**

给定一个数组，其中每个元素出现两次，除了一个元素。目标是找到那个只出现一次的元素，并且在常数额外空间中完成。

**示例 1**

```go
Input: [2, 1, 2, 3, 3]
Output: 1
```

**示例 2**

```go
Input: [1, 1, 4]
Output: 4
```

这个方法是使用异或运算。我们将利用异或的两个性质。

+   一个数字与其自身进行异或运算结果为 0

+   0 与任何数字进行异或运算，结果是该数字

所以，方法是对数组中的所有数字进行异或运算。最后得到的数字就是答案。

# **程序**

以下是对应的程序

```go
package main

import "fmt"

func singleNumber(nums []int) int {
	lenNums := len(nums)
	res := 0
	for i := 0; i < lenNums; i++ {
		res = res ^ nums[i]
	}

	return res
}

func main() {
	output := singleNumber([]int{2, 1, 2, 3, 3})
	fmt.Println(output)

	output = singleNumber([]int{1, 1, 4})
	fmt.Println(output)

}
```

**输出：**

```go
1
4
```

同时，查看我们的系统设计教程系列：[系统设计教程系列](https://techbyexample.com/system-design-questions/)
