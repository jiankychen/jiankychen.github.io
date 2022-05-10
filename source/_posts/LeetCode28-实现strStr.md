---
title: LeetCode 28. 实现 strStr()
tags:
  - 字符串
  - KMP
categories:
  - LeetCode
cover: false
abbrlink: 8f083358
date: 2022-05-10 19:30:57
---

[LeetCode 28. Implement strStr()](https://leetcode.cn/problems/implement-strstr/)


Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

**Clarification:**

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return `0` when `needle` is an empty string. This is consistent to C's `strstr()` and Java's `indexOf()`.

 

**Example 1:**

    Input: haystack = "hello", needle = "ll"
    Output: 2


**Example 2:**

    Input: haystack = "aaaaa", needle = "bba"
    Output: -1
 

**Constraints:**

 - $1 \le$ `haystack.length`, `needle.length` $\le 10^4$
 - `haystack` and `needle` consist of only lowercase English characters.

## 思路

本题是经典的字符串单模匹配的模型，可以使用字符串匹配算法解决，常见的字符串匹配算法包括暴力匹配、Knuth-Morris-Pratt 算法、Boyer-Moore 算法、Sunday 算法等

[力扣官方题解：实现 strStr()](https://leetcode.cn/problems/implement-strstr/solution/shi-xian-strstr-by-leetcode-solution-ds6y/)

## Method 1: 暴力搜索

基本思路：让字符串 `needle` 与字符串 `haystack` 中所有长度为 `m` 的子串均匹配一次，其中，`m` 为字符串 `needle` 的长度

```cpp
// 检查字符串 haystack 中以索引 start 开头的一连串字符是否与 needle 字符串匹配
bool isSubStr(string haystack, int start, string needle) {
    for (auto c : needle) {
        if (haystack[start] != c)
            return false;
        start++;
    }
    return true;
}

int strStr(string haystack, string needle) {
    for (int i = 0; i < haystack.size(); i++) {
        if (isSubStr(haystack, i, needle))
            return i; 
    }
    return -1;
}
```

时间复杂度：最坏情况下为 $O(n \times m)$，`n` 为字符串 `haystack` 的长度，`m` 为字符串 `needle` 的长度

空间复杂度：$O(1)$


## Method 2: KMP 算法

Knuth-Morris-Pratt 算法，简称 KMP 算法，由 Donald Knuth、James H. Morris 和 Vaughan Pratt 三人于 1977 年联合发表

参考：
 - [力扣官方题解：实现 strStr()](https://leetcode.cn/problems/implement-strstr/solution/shi-xian-strstr-by-leetcode-solution-ds6y/)
 - [代码随想录：实现 strStr()](https://www.programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html)