---
title: 数组中两个不重合切片的和的最大值
date: 2019-08-18 20:15:18
tags:
  - 算法
layout: post
---

_这道题困扰了我一整天，有必要记录一下这个解题思路。_

# 题目描述

原题地址：<https://leetcode.com/problems/maximum-sum-of-two-non-overlapping-subarrays/>

在一个数组中，寻找一段长为`L`的切片，与一段长为`M`的切片，两个切片是不重合的，求两个切片的和的最大值。

输入：数组`nums`，整数`L`与`M`

输出：和的最大值

# 解题方法

解题法来自一位小伙伴的post：<https://leetcode.com/problems/maximum-sum-of-two-non-overlapping-subarrays/discuss/278251/JavaC%2B%2BPython-O(N)Time-O(1)-Space>

总的来说，分成两大步骤。

## 步骤一：前缀和

首先，为了能够快速求得切片的和，不是每一次都将切片加和，利用前缀和。即保存一个数组，其`i`位置的元素，是`[0, i]`的所有元素的总和。那么，若要求`[a, b]`这个切片的和，我们就只需要用到一个减法即可，如下。

```
Sum[a, b] = num[a] + num[a + 1] + ... + num[b]
          = num[0] + num[1] + ... + num[a - 1] + num[a] + num[a + 1] + ... + num[b] - (num[0] + num[1] + ... + num[a - 1])
          = Sum[0, b] - Sum[0, a - 1]
          = PrefixSum[b] - PrefixSum[a - 1]
```

两个数相减就得到子切片的和。

那么这个建立前缀和的步骤，时间上只需要`O(n)`即可，遍历一次数组即可。

这算得上是前缀和的一个用途。

## 步骤二 遍历

算法真正牛的地方在这里。

题目中，L切片和M切片（叙述方便，把`L`长度切片称为L切片）的谁在前谁在后是没有规定的。那么下面的描述，假设L切片在前，M切片在后。另一种情况是同理的，且取两种情况的最大值返回即可。

算法遍历一次数组，在第`i`次的遍历中，那么我假定选择`num[i - M + 1, i]`，也就是当前已经遍历到的这段数组的最后那长度为M的切片，选定为M。M是否最大的其实我们不知道，但是我们关心的只是最大的和。但是同时，我们选定`num[i - M - L + 1, i - M]`为L切片，计算其和，然后与前面循环中选择的L切片作为比较，保存最大的L切片的和。

这样我们每一次的遍历，都会得到当前已知的，不与我所要选择的M切片重合的L切片的最大和，然后再与这个M切片相比较，那么假如我们前面得到了一个最大的L切片，当前位置恰好是最大的M切片，我们就得到的最大的和。

另一种情况也类似，我们只不过当前最后的切片为L切片，与前面最大的M切片和做比较。

然后，比较两个情况，取其最大值即可，一轮循环结束。

# 实现

使用go语言的实现如下

```go
func maxSumTwoNoOverlap(A []int, L int, M int) int {
	// 前缀和
	for i := 1; i < len(A); i++ {
		A[i] += A[i-1]
	}
	res := A[L+M-1]
	// 初始化，认为从第一个开始的子数组是最大的
	Lmax := A[L-1]
	Mmax := A[M-1]
	max := func(i1, i2 int) int {
		if i1 > i2 {
			return i1
		} else {
			return i2
		}
	}

	for i := L + M; i < len(A); i++ {
		// 每一轮，LMax都更新为从i-L-M开始的L个数字的最大，也就是[0,i-L-M]中最大的L长度的子数组
		Lmax = max(Lmax, A[i-M]-A[i-L-M])
		// 同上
		Mmax = max(Mmax, A[i-L]-A[i-L-M])

		// 然后，这个最长的L的子数组加上从i-M开始的M长度的子数组，这样L和M子数组是不重合的，并且取得了L的最大值。
		// 没有看M是否最大，但是若是，则res就成了最大的了
		res = max(res, max(Lmax+A[i]-A[i-M], Mmax+A[i]-A[i-L]))
	}
	return res
}


```
