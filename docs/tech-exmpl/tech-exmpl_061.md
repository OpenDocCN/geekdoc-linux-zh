# 房屋抢劫问题的程序

> 原文：[`techbyexample.com/program-for-house-robber-problem/`](https://techbyexample.com/program-for-house-robber-problem/)

## **概述**

附近有几所房屋。每所房屋里都有一些钱。这些房屋用一个数组表示，数组中的每个条目表示该房屋里的钱数。

例如，如果我们有以下数组

```go
[2, 3, 4, 2]
```

然后

+   **第一**所房屋有**2**钱

+   **第二**所房屋有**3**钱

+   **第三**所房屋有**4**钱

+   **第四**所房屋有**2**钱

盗贼可以抢劫任意数量的房屋，但不能抢劫两个相邻的房屋。例如，他可以在以下组合中抢劫上述数组中的房屋

+   1 和 3

+   1 和 4

+   2 和 4

上述组合中没有相邻的房屋。问题是找出可以为盗贼带来最大收益的组合。

例如，在上述情况下，第一个组合（1 和 3）将给盗贼带来最大的收益，即 2+4=6，因此盗贼可以抢劫第一和第三所房屋，即 2+4=6

另一个例子

```go
Input: [1, 6, 8, 2, 3, 4]
Output: 13
```

盗贼可以抢劫第一、第三和第六所房屋，即 1+8+4=13

这是一个动态规划问题，因为它具有最优子结构。假设这个数组的名字是**money**

+   dp[0] = money[0]

+   dp[1] = max(money[0], money[1])

+   dp[2] = max(money[0] + money[1], money[2])

+   dp[i] = dp[i] + max(dp[i-1], dp[i-1])

其中 **dp[i]** 表示如果包括第 i 所房屋，盗贼可以抢到的钱数。最后，我们返回 **dp** 数组中的最大值

## **程序**

这是相应的程序。

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
