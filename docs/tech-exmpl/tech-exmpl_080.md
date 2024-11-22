# 跳跃游戏程序

> 原文：[`techbyexample.com/jump-game-program/`](https://techbyexample.com/jump-game-program/)

## 概述

提供了一个输入数组。数组中的每个条目表示从该位置开始的最大跳跃长度。要求从第一个索引开始，如果能到达最后一个索引则返回 true，如果无法到达最后一个索引则返回 false。

示例

```go
Input: [2, 3, 1, 1, 4]
Output: true

Input: [3, 2, 1, 0, 4]
Output: false
```

在第一个示例中，有多种方式可以到达最后一个索引。

+   0-1-4

+   0-2-3-4

在第二个示例中，没有办法到达最后一个索引。你能到达的最远位置是倒数第二个索引。由于倒数第二个索引的值为零，因此无法到达最后一个索引。

还有一种变种是需要返回最小跳跃次数。稍后我们将查看该程序。

## **程序**

```go
package main

import "fmt"

func canJump(nums []int) bool {
	lenNums := len(nums)
	canJumpB := make([]bool, lenNums)

	canJumpB[0] = true

	for i := 0; i < lenNums; i++ {

		if canJumpB[i] {
			valAtCurrIndex := nums[i]
			for k := 1; k <= valAtCurrIndex && i+k < lenNums; k++ {
				canJumpB[i+k] = true
			}
		}

	}

	return canJumpB[lenNums-1]
}

func main() {
	input := []int{2, 3, 1, 1, 4}

	canJumpOrNot := canJump(input)
	fmt.Println(canJumpOrNot)

	input = []int{3, 2, 1, 0, 4}

	canJumpOrNot = canJump(input)
	fmt.Println(canJumpOrNot)

}
```

**输出**

```go
true
false
```

还有一种变种是需要返回最小跳跃次数。下面是对应的程序。

```go
package main

import "fmt"

func jump(nums []int) int {

	lenJump := len(nums)
	minJumps := make([]int, lenJump)
	for i := 0; i < lenJump; i++ {
		minJumps[i] = -1
	}

	minJumps[0] = 0

	for i := 0; i < lenJump; i++ {
		currVal := nums[i]

		for j := 1; j <= currVal && i+j < lenJump; j++ {
			if minJumps[i+j] == -1 || minJumps[i+j] > minJumps[i]+1 {
				minJumps[i+j] = minJumps[i] + 1
			}
		}
	}

	return minJumps[lenJump-1]

}

func main() {
	input := []int{2, 3, 1, 1, 4}

	minJump := jump(input)
	fmt.Println(minJump)

	input = []int{3, 2, 1, 0, 4}

	minJump = jump(input)
	fmt.Println(minJump)

}
```

**输出**

```go
2
-1
```

+   [游戏](https://techbyexample.com/tag/game/)*   [跳跃](https://techbyexample.com/tag/jump/)
