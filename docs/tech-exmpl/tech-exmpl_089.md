# 组合求和问题的程序

> 原文：[`techbyexample.com/program-combination-sum-problem/`](https://techbyexample.com/program-combination-sum-problem/)

## **概述**

给定一个整数数组和一个目标数字，目标是找到数组中所有能够和为目标数字的组合数。

数组中的每个元素可以在同一组合中使用任意次数

示例

```go
Input: [3,4,10,11]
Target: 10
Output: [[3,3,4],[10]]
```

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func combinationSum(candidates []int, target int) [][]int {
	lengthCandidates := len(candidates)
	current_sum_array := make([]int, 0)
	output := make([][]int, 0)
	combinationSumUtil(candidates, lengthCandidates, 0, 0, 0, target, current_sum_array, &output)
	return output
}

func combinationSumUtil(candidates []int, lengthCandidates, index, current_sum_index, current_sum, target int, current_sum_array []int, output *[][]int) {

	if index >= lengthCandidates {
		return
	}

	if current_sum > target {
		return
	}

	if current_sum == target {
		var o []int
		for i := 0; i < current_sum_index; i++ {
			o = append(o, current_sum_array[i])
		}
		*output = append(*output, o)
		return
	}

	//Exclude
	combinationSumUtil(candidates, lengthCandidates, index+1, current_sum_index, current_sum, target, current_sum_array, output)

	//Include
	current_sum_array = append(current_sum_array, candidates[index])

	combinationSumUtil(candidates, lengthCandidates, index, current_sum_index+1, current_sum+candidates[index], target, current_sum_array, output)
}

func main() {
	output := combinationSum([]int{3, 4, 10, 11}, 10)
	fmt.Println(output)
}
```

**输出**

```go
[[10] [3 3 4]]
```
