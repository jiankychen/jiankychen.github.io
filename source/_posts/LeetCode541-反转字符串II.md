---
title: LeetCode 541. 反转字符串 II
tags:
  - 双指针
  - 字符串
  - 模拟
categories:
  - LeetCode
cover: false
abbrlink: bd781faa
date: 2022-05-07 21:45:31
---

[LeetCode 541. Reverse String II](https://leetcode-cn.com/problems/reverse-string-ii/)


Given a string `s` and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.

If there are fewer than `k` characters left, reverse all of them. If there are less than `2k` but greater than or equal to `k` characters, then reverse the first `k` characters and leave the other as original.

 

**Example 1:**

    Input: s = "abcdefg", k = 2
    Output: "bacdfeg"


**Example 2:**

    Input: s = "abcd", k = 2
    Output: "bacd"
 

**Constraints:**

 - $1 \le$ `s.length` $\le 10^4$
 - `s` consists of only lowercase English letters.
 - $1 \le$ `k` $\le 10^4$


## 思路

直接按题意进行模拟：

反转每个下标从 `2k` 的倍数开始的、长度为 `k` 的子串，若该子串长度不足 `k`，则反转整个子串

## Method: 双指针

解题步骤：

将字符串 `s` 分成若干段长为 `2k` 的区间（最后一段不足 `2k` 也同样视为一个区间），针对每个区间执行以下操作
 - 若区间内的字符多于 `k` 个，反转前 `k` 个字符，反转操作与 [LeetCode 344. 反转字符串](https://jiankychen.github.io/posts/d9baf118/) 一致
 - 若区间内的字符少于 `k` 个，反转所有字符

其中，针对区间的遍历，只需遍历区间的起点，即，以 `i = 0` 为起点，每次按照 `i = i + 2 * k` 更新 `i` ，当 `i >= s.size()` 时结束遍历

代码实现：

```cpp
// 反转 left 到 right 之间的字符
void reverse(string &s, int left, int right) {
    while (left < right)
        swap(s[left++], s[right--]);
}

// 反转字符串
string reverseStr(string s, int k) {
    for (int i = 0; i < s.size(); i += 2 * k) { // 每 2k 个字符，进行一次 前 k 个字符的反转
        if (i + k - 1 < s.size()) {  // 剩余字符多于 k 个，反转前 k 个字符
            reverse(s, i, i + k - 1);
            continue;
        }
        reverse(s, i, s.size() - 1); // 剩余字符少于 k 个，将全部剩余字符反转
    }
    return s;
}
```

特别地，可以将上述 `for` 循环写成：

    for (int i = 0; i < s.size(); i += 2 * k)
        reverse(s, i, min(i + k - 1, num.size() - 1));


时间复杂度：$O(n)$，其中，$n$ 为字符串长度
 - 遍历 `i` 的时间复杂度为 $O(n / k)$
 - 每反转 `k` 个字符的时间复杂度为 $O(k)$

空间复杂度：$O(1)$

> 当需要以固定规律对字符串逐段处理时，可试着在 `for` 循环的表达式上做做文章

参考：[力扣官方题解：反转字符串 II](https://leetcode-cn.com/problems/reverse-string-ii/solution/fan-zhuan-zi-fu-chuan-ii-by-leetcode-sol-ua7s/)