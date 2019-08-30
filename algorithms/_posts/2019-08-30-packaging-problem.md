---
title: 装袋问题——二分搜索的另一种用途
date: 2019-08-30 20:23:33
tags:
  - 算法
  - 二分搜索
layout: post
---

# 题目描述

[LeetCode 1011](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)题目，我将这类型的题目称为装袋问题，他们的一般性描述如下

按顺序将`i`件货物装入到承重固定的袋子中，每一件的货物重量为`weights[i]`，若袋子的数量是`N`，求出袋子承重量的最小值。

**输入**：`int[] weights, N`
**输出**：袋子最小承重量

# 解法

我们看到，`weights`数组不一定是排好序的，那何来的二分搜索呢？

这里的二分搜索，不是传统的在一个数组里面搜索，而是利用二分的方法，找出一个某个给定范围中的最小值。要知道，给定一个重量`w`，求出需要多少个袋子是很容易的。那么，我就可以在这个范围中，用二分的方法，找出这个最小的满足袋子数为`N`的`w`。

这个范围是怎么定的呢？起始点至少要等于`weights`数组中的最大值，而终止点则应该是我们能找的最大的合理的数目，也就是我们将所有的货物都放进1个袋子时候的重量，这个值就是我们的终止点。

剩下的，就跟二分算法差不多了。每次取中间的值，看这个值是否是满足`N`个袋子的最小的`w`即可。

# 1011题目源代码

```go
func shipWithinDays(weights []int, D int) int {
	begin := 0
	end := 0
	for i := 0; i < len(weights); i++ {
		if weights[i] > begin {
			begin = weights[i]
		}
		end += weights[i]
	}

	for begin < end {
		capacity := begin + (end-begin)/2
		cur := 0
		count := 0
		for i := 0; i < len(weights); i++ {
			if cur+weights[i] > capacity {
				count++
				cur = 0
			}
			cur += weights[i]
		}
		count++
		if count > D {
			begin = capacity + 1
		} else {
			end = capacity
		}
	}
	return end
}

```

# 感想

这道题想了我真的久，看到题目属于二分算法的范畴，就一直想搜索的数组在哪里？于是先想到了前缀和，因为前缀和一定是排好序的。一开始的算法就是用二分搜索在前缀和里面找`D`次，如果找到数组最后一个值，说明当前搜索的重量够，如果没有，则不够。这样一来最后的实现用了两次二分，还是很慢。看了题解之后，才知道原来二分搜索这么巧妙。

真实榆木脑袋。
