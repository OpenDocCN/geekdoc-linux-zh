# 将字谜分组的程序

> 原文：[`techbyexample.com/group-anagrams-together-program/`](https://techbyexample.com/group-anagrams-together-program/)

## **概述**

给定一个字符串数组，编写一个程序将所有字谜分组。来自维基百科

**字谜** 是通过重新排列另一个单词或短语的字母形成的单词或短语，通常使用所有原始字母且每个字母恰好使用一次。例如，单词*anagram*本身可以重新排列成*nag a ram*，单词*binary*可以重新排列成*brainy*，单词*adobe*可以重新排列成*abode*。

例如

```go
Input: ["art", "tap", "rat", "pat", "tar","arm"]
Output: [["art", "rat", "tar"], ["tap", "pat"], ["arm"]]
```

以下是策略。

+   复制原始数组。对每个字符串进行排序，复制数组将如下所示

```go
["art", "apt", "art", "apt", "art", "arm"]
```

+   创建一个映射来存储输出

```go
var output map[string][]int
```

+   为上述复制数组构建一个字典树，所有字符串都已排序。在插入每个元素后更新上述映射。对于“art”，映射应如下所示，因为 art 在原始数组中的字谜分别位于 0、2 和 5 的位置。

```go
map["art"] = [0,2,4]
```

+   遍历映射，并通过索引输入字符串数组来打印输出

## **程序**

以下是相应的程序

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	strs := []string{"art", "tap", "rat", "pat", "tar", "arm"}
	output := groupAnagrams(strs)
	fmt.Println(output)

	strs = []string{""}
	output = groupAnagrams(strs)
	fmt.Println(output)

	strs = []string{"a"}
	output = groupAnagrams(strs)
	fmt.Println(output)
}

type sortRune []rune

func (s sortRune) Swap(i, j int) {
	s[i], s[j] = s[j], s[i]
}

func (s sortRune) Less(i, j int) bool {
	return s[i] < s[j]
}

func (s sortRune) Len() int {
	return len(s)
}

func groupAnagrams(strs []string) [][]string {

	anagramMap := make(map[string][]int)
	var anagrams [][]string
	trie := &trie{root: &trieNode{}}

	lenStrs := len(strs)

	var strsDup []string

	for i := 0; i < lenStrs; i++ {
		runeCurrent := []rune(strs[i])
		sort.Sort(sortRune(runeCurrent))
		strsDup = append(strsDup, string(runeCurrent))
	}

	for i := 0; i < lenStrs; i++ {
		anagramMap = trie.insert(strsDup[i], i, anagramMap)
	}

	for _, value := range anagramMap {
		var combinedTemp []string
		for i := 0; i < len(value); i++ {
			combinedTemp = append(combinedTemp, strs[value[i]])
		}
		anagrams = append(anagrams, combinedTemp)
	}

	return anagrams
}

type trieNode struct {
	isWord    bool
	childrens [26]*trieNode
}

type trie struct {
	root *trieNode
}

func (t *trie) insert(input string, wordIndex int, anagramMap map[string][]int) map[string][]int {
	inputLen := len(input)
	current := t.root

	for i := 0; i < inputLen; i++ {
		index := input[i] - 'a'
		if current.childrens[index] == nil {
			current.childrens[index] = &trieNode{}
		}
		current = current.childrens[index]
	}
	current.isWord = true
	if anagramMap[input] == nil {
		anagramMap[input] = []int{wordIndex}
	} else {
		anagramMap[input] = append(anagramMap[input], wordIndex)
	}
	return anagramMap
}
```

**输出**

```go
[[art rat tar] [tap pat] [arm]]
[[]]
[[a]]
```
