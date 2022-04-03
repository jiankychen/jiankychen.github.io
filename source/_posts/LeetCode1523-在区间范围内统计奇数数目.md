---
title: LeetCode 1523. 在区间范围内统计奇数数目
date: 2022-03-15 22:20:34
tags:
 - 数组
categories:
 - LeetCode
cover: false
---

[LeetCode 1523](https://leetcode-cn.com/problems/count-odd-numbers-in-an-interval-range/)

Given two non-negative integers `low` and `high`. Return **the count of odd numbers** between `low` and `high` (inclusive).

 

Example 1:

    Input: low = 3, high = 7
    Output: 3
    Explanation: The odd numbers between 3 and 7 are [3,5,7].


Example 2:

    Input: low = 8, high = 10
    Output: 1
    Explanation: The odd numbers between 8 and 10 are [9].



## Method: 前缀和思想
> 暴力枚举 $[low,high]$ 中的所有元素会超出时间限制。

可以使用前缀和思想来解决这个问题，定义 $pre(x)$ 为区间 $[0,x]$ 中奇数的个数，则 $pre(x)=(x+1)/2$。因此 $[low,high]$ 之间的奇数数目为 $pre(high) − pre(low−1)$

```cpp
int pre(int x){ // 返回[0,x]之间的奇数数目
    return (x + 1) / 2;
}

int countOdds(int low, int high) { // [low,high]之间的奇数数目=[0,high]之间的奇数数目-[0,low-1]之间的奇数数目
    return pre(high) - pre(low - 1);
}
```

时间复杂度：`O(1)`。

空间复杂度：`O(1)`。