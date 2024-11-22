# 最佳买卖股票时间程序

> 原文：[`techbyexample.com/best-time-buy-sell-stocks-program/`](https://techbyexample.com/best-time-buy-sell-stocks-program/)

## **概述**

给定一个数组**prices**，其中**prices[i]**表示第 i 天的股票价格。你只能进行一次买卖。找到通过一次买卖股票所能获得的最大利润。

示例

```go
Input: [4,2,3,8,1]
Output: 6
Buy on the second day at 2 and sell on the 4th day at 8\. Profit = 8-2 = 6
```

原始顺序应保持不变。

## **程序**

这是相应的程序。

```go
package main

import "fmt"

func maxProfit(prices []int) int {
	lenPrices := len(prices)

	buy := -1
	sell := -1

	maxProphit := 0

	for i := 0; i < lenPrices; {
		for i+1 < lenPrices && prices[i] > prices[i+1] {
			i++
		}

		if i == lenPrices-1 {
			return maxProphit
		}

		buy = i

		i++

		for i+1 < lenPrices && prices[i] < prices[i+1] {
			i++
		}

		sell = i

		if (prices[sell] - prices[buy]) > maxProphit {
			maxProphit = prices[sell] - prices[buy]
		}
		i++
	}

	return maxProphit
}

func main() {
	output := maxProfit([]int{4, 2, 3, 8, 1})
	fmt.Println(output)
}
```

**输出**

```go
6
```
