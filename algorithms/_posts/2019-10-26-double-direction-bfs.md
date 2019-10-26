---
title: 双向BFS——有效降低内存消耗的搜索算法
date: 2019-10-26 11:41:14
tags:
  - 算法
  - 图
  - 广度遍历
layout: post
---

# 问题描述

首先祭出本文所讨论的算法题：[LeetCode 127 单词接龙](https://leetcode-cn.com/problems/word-ladder/)

给定两个单词（beginWord 和 endWord）和一个字典，找到从 beginWord 到 endWord 的最短转换序列的长度。转换需遵循如下规则：

每次转换只能改变一个字母。**转换过程中的中间单词必须是字典中的单词**。说明:

- 如果不存在这样的转换序列，返回 0。
- 所有单词具有相同的长度。
- 所有单词只由小写字母组成。
- 字典中不存在重复的单词。
- 你可以假设 beginWord 和 endWord 是非空的，且二者不相同。

示例 1:

```
输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出: 5

解释: 一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog",
     返回它的长度 5。
```


示例 2:
```
输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: 0

解释: endWord "cog" 不在字典中，所以无法进行转换。
```
**著作权归领扣网络所有**

# 问题分析

起初看到词汇表的时候，我想到的是使用前缀树，加上之前做过一道题，使用前缀树匹配至多有一个字母不同的所有单词，于是乎一开始下手先使用前缀树解决。但是，这个方法过慢，因为每次遍历节点的时候，我有两个选择：1）查找匹配当前字符的节点。2）查找不匹配当前字符的其他节点（当且仅当前面匹配都是正确的时候）。第二个选择的实现，需要遍历整个节点的子节点。于是，对于一个单词的查找，几乎把整棵树都给遍历了一遍，因此这个算法毫不意外的超时了。

于是重新想另一个方法，可以观察到我们查找是从一个单词转换为另一个单词，就相当于从图上的一个节点跳转到另一个节点，可否跳转的条件，就是这两个节点仅有一个字母的不同。

为了快速找到能够跳转的下一个节点，需要预先处理`wordList`。对于一个单词，比如说`hit`，那么就有3个位置能够改变，变成单词表中的其他字母的位置，如中间的`i`变成`o`。那么可以发现，`hit`与`hot`两个节点是相连的，并且均有`h*t`的形式。于是，我们可以预先构建一个表，把所有单词中，每次替换一个字符为`*`，如`*it`, `h*t`, `hi*`，保存到哈希表中，就能够快速找到某个单词能够变换的下一个单词了。

得到了变换表之后，我们就可以开始遍历查找了。题目中要求求最短的路径，很容易就想到应该使用广度遍历搜索，逐级展开。

但是，这就引入了另一个问题，当词汇表变得很大的时候，我们将会在广度遍历的队列中加入非常多的待遍历节点，并且节点是以指数级增加的，因而会内存一定会爆满，而算法还没有结束。

为了减少内存的消耗，我们需要优化。可以看到，这个题目我们是知道结束的单词的，所以其实我们也可以同时从结束那一端出发，往开始的位置前进。理想条件下，我们仅需要展开2棵深度为原本最短路径的一半的树，比原本展开的树小了非常多（n次方的多叉树变成了两颗n/2的多叉树）。

因此，实现的时候，我们每次先查看正向的BFS遍历，然后是反向的BFS遍历，最后遍历到一个节点已经被另一个方向的遍历访问过的时候，找到最短的路径，算法结束。

# 实现注意点

## 先设置Visit哈希表，再实际访问节点

一般我实现BFS的时候，都习惯一股脑把子节点加入队列。遍历节点，先查看是否已经访问过，没有的时候则遍历节点，并设置visit。类似如下的代码

```go
if !visited[node] {
  visited[node] = true
  for i := 0; i < len(node.next); i++ {
    queue = append(queue, node.next[i])
  }
}
```

但是在这里，这种实现就出现了问题（不过好像原本这么实现也会有一些多余的节点产生），并且这个问题非常隐蔽，以至于我的实现通过了38个用例，之后才出错。

### 一个出错的例子

考虑下面这一种情况。

假设在某次遍历中，正向BFS到达p节点，其能够转移到b、d两个节点，q能够到达d、f两个节点，而b能够到达f，假设队列变成如下的样子，`.`代表其他不考虑的至少1个的节点。

```
前向队列：p|.............bd
后向队列：q|...df.....b....
          ^
```

反向BFS都访问了d、f与b，且都设置了visited为true。然后又过了几个循环，正向BFS才访问到b，发现b已经在反向BFS中访问过了，于是返回，得到答案`...,p,b,f,q,..`，但是很明显，`...,p,d,q,...`才是正确答案，多出了一个节点。

### 如果先设置visited

如果在p节点时，事先设置了b、d两节点的visited，那么，在反向遍历到d的时候，就发现前向已经访问了d，算法立即返回，得到正确答案`...,p,d,q,...`。

### 错误产生的原因

根据个人不负责任的推理，在一个方向的遍历上，我可能当前访问的节点，还在另一个方向的队列里面，而队列的排列，对不同层是有先后次序，但是对于同一层的节点，顺序是随机的，这就导致了我可能先访问到了更长路径的节点。

正向访问中，d一定比f先访问，反向中d一定比b先访问。无论哪边先实际访问d，另一边的visited[d]都已经为true，因此能够立即返回正确结果。但是如果不事先设置visited[d]为true，就可能会因为另一边访问到d时，没有发现已经这边已经到了d，而继续访问后面的错误的结果，就像错误例子里面展示的那样。

（为了说服自己，花了整整一个下午。。。）

# 祭出我的源码

```go
package leetcode

func ladderLength(beginWord string, endWord string, wordList []string) int {
	wordMap := make(map[string][]string)
	foundEnd := false
	for i := 0; i < len(wordList); i++ {
		if endWord == wordList[i] {
			foundEnd = true
		}
		buf := []byte(wordList[i])
		for j := 0; j < len(buf); j++ {
			ori := buf[j]
			buf[j] = '*'
			key := string(buf)
			wordMap[key] = append(wordMap[key], wordList[i])
			buf[j] = ori
		}
	}
  // 如果结束单词不存在的话，立即返回0
	if !foundEnd {
		return 0
	}

	type bfsContext struct {
		word  string
		count int
	}

	// 双向广度优先遍历
	forwardVisited := make(map[string]int)
	forwardVisited[beginWord] = 1
	backwardVisited := make(map[string]int)
	backwardVisited[endWord] = 1
	forwardQueue := make([]*bfsContext, 0, 100)
	backwardQueue := make([]*bfsContext, 0, 100)
	forwardQueue = append(forwardQueue, &bfsContext{
		word:  beginWord,
		count: 1,
	})
	backwardQueue = append(backwardQueue, &bfsContext{
		word:  endWord,
		count: 1,
	})

	visitNode := func(queue *[]*bfsContext, visited map[string]int, otherVisited map[string]int) int {
		c := (*queue)[0]
		*queue = (*queue)[1:]

		//visited[c.word] = c.count

		buf := []byte(c.word)
		for i := 0; i < len(c.word); i++ {
			ori := buf[i]
			buf[i] = '*'
			key := string(buf)
			for _, word := range wordMap[key] {
				if otherVisited[word] != 0 {
					// 前后向都找到了
					return c.count + otherVisited[word]
				}

				if visited[word] == 0 {
					visited[word] = c.count + 1
					*queue = append(*queue, &bfsContext{
						word:  word,
						count: c.count + 1,
					})
				}
			}
			buf[i] = ori
		}
		return -1
	}

	for len(forwardQueue) > 0 && len(backwardQueue) > 0 {
		node := visitNode(&forwardQueue, forwardVisited, backwardVisited)
		if node != -1 {
			return node
		}

		node = visitNode(&backwardQueue, backwardVisited, forwardVisited)
		if node != -1 {
			return node
		}
	}

	return 0
}
```
