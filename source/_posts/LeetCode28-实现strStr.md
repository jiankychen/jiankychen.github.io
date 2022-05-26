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

时间复杂度：$O(n \times m)$，其中，$n$ 为字符串 `haystack` 的长度，$m$ 为字符串 `needle` 的长度

空间复杂度：$O(1)$


## Method 2: KMP 算法

文本串为 `haystack` ，模式串为 `needle` ，利用 KMP 算法进行字符串匹配

算法流程：

1. 构造 next 数组，用于查找回退位置

2. 利用 next 数组进行匹配，定义 `i` 为文本串索引，`j` 为模式串索引，执行循环：
    - 若模式串字符 `needle[j]` 与文本串字符 `haystack[i]` 相同，则将 `i` 和 `j` 同时右移
    - 若不相同，连续执行 `j = next[j - 1]` ，直到 `needle[j] == haystack[i]` 或 `j == 0`

具体原理及步骤可参阅 [KMP 算法](https://jiankychen.github.io/posts/36b55f59/)

代码实现：

```cpp
// 计算 next 数组
void getNext(vector<int> &next, string s) {
    int prefix = 0;
    next[0] = 0;
    for (int suffix = 1; suffix < s.size(); suffix++) {
        while (prefix > 0 && s[prefix] != s[suffix])
            prefix = next[prefix - 1];
        if (s[prefix] == s[suffix])
            prefix++;
        next[suffix] = prefix;
    }
}

// 字符串匹配
int strStr(string haystack, string needle) {
    vector<int> next(needle.size(), 0);
    getNext(next, needle);
    int j = 0;
    for (int i = 0; i < haystack.size(); i++) {
        while (j > 0 && needle[j] != haystack[i]) // needle[j] 与 haystack[i] 不同，回退 j
            j = next[j - 1];
        if (needle[j] == haystack[i])       // needle[j] 与 haystack[i] 相同，右移 i 和 j
            j++;
        if (j == needle.size())             // 匹配完成
            return (i - needle.size() + 1); // 正确匹配的起点索引为 i - needle.size() + 1
    }
    return -1;
}
```

时间复杂度：$O(n + m)$

空间复杂度：$O(m)$，使用额外空间存储 next 数组


参考：
 - [代码随想录](https://www.programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html)
 - [力扣官方题解](https://leetcode.cn/problems/implement-strstr/solution/shi-xian-strstr-by-leetcode-solution-ds6y/)