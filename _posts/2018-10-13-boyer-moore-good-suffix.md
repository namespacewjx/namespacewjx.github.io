---
layout:     post
title:      "BoyerMoore字符串匹配算法好后缀表构建"
author:     "吴俊贤"
tags:
    - 算法
    - 字符串
---

*书里面对好后缀的构建讲的内容太少，实现算法的时候发现有许多的问题，因此在这里详细介绍一些应该如何构造好后缀表。*

# 符号说明

- l：字符串的长度
- `goodSuffix`：好后缀表数组名

# 后缀表数组

好后缀表可以用一个长度为l的数组来存着，暂且称之为`goodSuffix`数组，那么`goodSuffix[i]`就是我们要查的好后缀为i时候的移动值。`goodSuffix[0]`初始化为l。

# 构造开始

我们从i为1开始，构建好后缀表。

## 判断1

我们试图去找i-1时候的那条好后缀，它的前面的匹配的字符串，在这条字符串前面的一个位置的字符串，是不是与i时候，新增加的那个字符相等。

举例来说，ABABAB这条字符串，我进行到i=3的时候，我找不到匹配了，然后我检查i=2的时候的好后缀，其匹配的是下标2-3的AB。i=3的时候新增了B，于是我看看2-3的AB前面的一个字符，刚好也是B，所以i=3时候的BAB，根据i=2的结果，找到了i=3时候的匹配字符串，`goodSuffix[3]`就等于`goodSuffix[2]`。

当然，`goodSuffix[0]`初始化为长度，i=1的时候是没有匹配的，这个情况是不会符合的，进入判断2。

## 判断2

我们试图寻找好后缀的除去好后缀之外的在字符串中的最后一个匹配。

通常字符串比较算法都是从字符串前面开始扫描，找到第一个匹配就停止的。我们要找最后一个匹配，所以我先把字符串反转过来，然后再开始寻找，那么这样就可以找到这个后缀，在这条字符串中的最后一个匹配的位置。

如果找到了这个匹配，则直接在`goodSuffix[i]`填入这个所需的移动值就行了。

否则进入判断3。

## 判断3

如果去掉了好后缀的第一个字符，是否在最前面有匹配呢？

如果有的话，那么我们可以移动字符串，就将去掉这个字符后的这个好后缀，对齐最前面匹配的这个前缀。

比如，在`CABDDDDCABEECAB`中，i为4的时候，`goodSuffix[3]`指代的下标为6-8的`CAB`，其前面是D，不匹配`ECAB`，不符合判断1。那么在前面也无法找到`ECAB`，不符合判断2。但是，我们去掉E之后，发现`CAB`在最前面有匹配。此时，因为这已经是字符串最前面了，再之前是啥都不用管，因此可以放心的移动字符串，将后面`CAB`的那个位置，对齐到前面的`CAB`。

对齐完了以后，`goodSuffix`后面的所有值，都是当前的这个值了。因为当前这个好后缀，在整条模式中，没有匹配的子串，随着i的增大，更加不会匹配。

如果没有，就彻底找不到了，我们就要移动这个字符串了，连同i+1之后的所有情况都是。

# go语言实现

```go
// 计算好后缀
table.goodSuffix = make([]int, len(pattern))
// 定义好后缀0为整条的长度
table.goodSuffix[0] = len(pattern)
//将字符串转过来
reversePattern := reverseString(pattern)
for i := 1; i < len(pattern); i++ {
  // 如果刚好i-1移动的位置，在这个好后缀里面
  // 此时我们需要比较，新加入的字符是不是匹配的
  if i > 1 && len(pattern)-table.goodSuffix[i-1]-(i-1)-1 >= 0 && pattern[len(pattern)-(i-1)-1] == pattern[len(pattern)-table.goodSuffix[i-1]-(i-1)-1] {
    // 如果匹配，则我们可以用上一个的值
    table.goodSuffix[i] = table.goodSuffix[i-1]
  } else if goodIndex := strings.Index(reversePattern[i:], reversePattern[:i]); goodIndex != -1 {
    // 借用库函数来寻找第一个好后缀。index是找到的这个好后缀在整条模式字符串中，从后面开始数过来的下标值。实际情况肯定不能用库函数，可以用一个匹配函数，只要不是这个就行，否则就成了递归了。
    table.goodSuffix[i] = goodIndex + i
  } else {
    // 我们只能寄希望字符串的长度为i-1的前缀，
    // 等于i-1时候的好后缀，如果不是，则彻底没有匹配，接下来都需要整条所有字符串
    if pattern[:i-1] == pattern[len(pattern)-(i-1):] {
      table.goodSuffix[i] = len(reversePattern) - (i - 1)
    } else {
      table.goodSuffix[i] = len(reversePattern)
    }

    // 这里只会进来一次，找不到一次，后面更不会找到，用i-1的值填入即可
    // 这个包含了i-1时在最前面有匹配的情况，此时也是用i-1的值填充接下来的所有值
    for i += 1; i < len(pattern); i++ {
      table.goodSuffix[i] = table.goodSuffix[i-1]
    }
    break
  }
}
```
