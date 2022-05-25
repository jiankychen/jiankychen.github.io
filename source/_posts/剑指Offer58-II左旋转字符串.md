---
title: 剑指Offer 58-II. 左旋转字符串
tags:
  - 字符串
  - 双指针
categories:
  - LeetCode
cover: false
abbrlink: 5c5f7907
date: 2022-05-10 18:50:22
---

[剑指 Offer 58-II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

The left-rotation operation of a string is to transfer a number of characters from the front of the string to the end of the string. Define a function that implements the left rotation of a string. For example, if you enter the string `"abcdefg"` and the number `2`, the function will return the result `"cdefgab"` rotated `2` places to the left.

**Example 1:**

    Input: s = "abcdefg", k = 2
    Output: "cdefgab"


**Example 2:**

    Input: s = "lrloseumgh", k = 6
    Output: "umghlrlose"

**Constraints:**
 - $1 \le$ `k` $\le$ `s.length` $\le 10000$

**Follow up:** Can you do this **in-place** with `O(1)` extra memory?

## 思路

解题思路：
1. 将整个字符串 `s` 反转
2. 将前 `s.size() - n` 个字符反转
3. 将后 `n` 个字符反转

以 `s = "abcdefg"` ，`n = 2` 为例：
 - 反转整个字符串，得到："gfedcba"
 - 反转前 5 个字符串，得到："cdefgba"
 - 反转后 2 个字符串，得到："cdefgab"

## Method: 双指针

采用双指针法实现字符串的反转

代码实现：

```cpp
void reverse(string &s, int left, int right) {
    while (left < right)
        swap(s[left++], s[right--]);
}

string reverseLeftWords(string s, int n) {
    reverse(s, 0, s.size() - 1);
    reverse(s, 0, s.size() - n - 1);
    reverse(s, s.size() - n, s.size() - 1);
    return s;
}
```

时间复杂度：$O(N)$，其中，$N$ 为字符串长度

空间复杂度：$O(1)$