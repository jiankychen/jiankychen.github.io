---
title: LeetCode 278. 第一个错误的版本
tags:
  - 二分查找
categories:
  - LeetCode
cover: false
abbrlink: 4cc2938
date: 2022-03-15 17:24:41
---

[LeetCode 278](https://leetcode-cn.com/problems/first-bad-version/)

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool `isBadVersion(version)` which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

 

**Example 1:**

    Input: n = 5, bad = 4
    Output: 4
    Explanation:
    call isBadVersion(3) -> false
    call isBadVersion(5) -> true
    call isBadVersion(4) -> true
    Then 4 is the first bad version.


**Example 2:**

    Input: n = 1, bad = 1
    Output: 1


## Method: 二分查找
注意到一个性质：当一个版本为正确版本，则该版本之前的所有版本均为正确版本；当一个版本为错误版本，则该版本之后的所有版本均为错误版本

将左右边界分别初始化为 `1` 和 `n` ，其中 `n` 是给定的版本数量。设定左右边界之后，每次我们都依据左右边界找到其中间的版本，检查其是否为正确版本。如果该版本为正确版本，那么第一个错误的版本必然位于该版本的右侧，我们缩紧左边界；否则第一个错误的版本必然位于该版本及该版本的左侧，我们缩紧右边界

这样我们每判断一次都可以缩紧一次边界，而每次缩紧时两边界距离将变为原来的一半，因此我们至多只需要缩紧 `O(logn)` 次

```cpp
int firstBadVersion(int n) {
    int left = 1, right = n;
    while (left < right) {  // 循环直至区间左右端点相同
        int mid = left + (right - left) / 2; // 防止计算时溢出
        if (isBadVersion(mid)) {
            right = mid;    // 答案在区间 [left, mid] 中
        } else {
            left = mid + 1; // 答案在区间 [mid+1, right] 中
        }
    }
    // 此时有 left == right，区间缩为一个点，即为答案
    return left;
}
```

时间复杂度：$O(\log{n})$，其中 `n` 是给定版本的数量

空间复杂度：$O(1)$。我们只需要常数的空间保存若干变量