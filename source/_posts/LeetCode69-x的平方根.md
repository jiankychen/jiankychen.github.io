---
title: LeetCode 69. x的平方根
date: 2022-03-14 19:22:34
tags:
 - 二分查找
categories:
 - LeetCode
cover: false
---

[LeetCode 69](https://leetcode-cn.com/problems/sqrtx/)

Given a non-negative integer `x`, compute and return the square root of `x`.

Since the return type is an integer, the decimal digits are truncated, and only **the integer part** of the result is returned.

Note: You are not allowed to use any built-in exponent function or operator, such as `pow(x, 0.5)` or `x ** 0.5`.

Example 1:

    Input: x = 4
    Output: 2


Example 2:

    Input: x = 8
    Output: 2
    Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.


## Method: 二分查找
找到满足`k*k <= x`的最大整数`k`
```cpp
int mySqrt(int x) {
    int left = 0, right = x;
    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        if ((long long) mid * mid <= x)
            left = mid + 1;
        else
            right = mid - 1;
    };
    return right;
}
```
其中，`(long long)`强制转换类型，避免`mid * mid`溢出


或，考虑`mid <= x / mid`（此时需另外分析`mid == 0`的情形）
```cpp
int mySqrt(int x) {
    int left = 0, right = x;
    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        if (mid == 0)
            break;
        else if (mid <= (x / mid))
            left = mid + 1;
        else
            right = mid - 1;
    };
    return right;
}
```