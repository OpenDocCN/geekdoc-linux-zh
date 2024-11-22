# 所得税计算程序

> 原文：[`techbyexample.com/program-income-tax-paid/`](https://techbyexample.com/program-income-tax-paid/)

# **概述**

给定一个二维数组，表示所得税的税率范围。输入数组是**brackets**，其中

```go
brackets[i] = [upperi, percenti]
```

这意味着第 i 个税率区间的上限是**upperi**，并按**percent**的税率征税。税率区间数组按上限排序。以下是税费的计算方法

+   upper0 以下的部分按 percent0 征税

+   upper1-upper0 按 percent1 征税

+   ……等等

你还将得到**收入**作为输入。你需要根据这个收入计算所得税。已知最后一个区间的上限大于收入。

**示例 1**

```go
Input: brackets = [[4,10],[9,20],[12,30]], income = 10
Output: 1.7
```

**示例 2**

```go
Input: brackets = [[3,10]], income = 1
Output: 0.3
```

# **程序**

以下是相应的程序

```go
package main

import "fmt"

func calculateTax(brackets [][]int, income int) float64 {
	if income == 0 {
		return 0
	}

	var totalTax float64

	numBrackets := len(brackets)
	upper := 0
	for i := 0; i < numBrackets; i++ {

		if i == 0 {
			upper = brackets[i][0]
		} else {
			upper = brackets[i][0] - brackets[i-1][0]
		}

		taxPer := brackets[i][1]
		if income <= upper {
			totalTax += float64(income) * float64(taxPer) / 100
			break
		} else {
			totalTax += float64(upper) * float64(taxPer) / 100
			income = income - upper
		}
	}

	return totalTax
}

func main() {
	output := calculateTax([][]int{{4, 10}, {9, 20}, {12, 30}}, 10)
	fmt.Println(output)

	output = calculateTax([][]int{{3, 10}}, 10)
	fmt.Println(output)

}
```

**输出：**

```go
1.7
0.3
```

此外，查看我们的系统设计教程系列：[系统设计教程系列](https://techbyexample.com/system-design-questions/)
