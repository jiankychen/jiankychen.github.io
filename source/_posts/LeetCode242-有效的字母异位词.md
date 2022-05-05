---
title: LeetCode 242. 有效的字母异位词
date: 2022-05-03 11:10:29
tags:
 - 哈希表
 - 数组
categories:
 - LeetCode
cover: false
---

[LeetCode 242. Valid Anagram](https://leetcode-cn.com/problems/valid-anagram/)

Given two strings `s` and `t`, return `true` if `t` is an **anagram** of `s`, and `false` otherwise.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

**Example 1:**

    Input: s = "anagram", t = "nagaram"
    Output: true


**Example 2:**

    Input: s = "rat", t = "car"
    Output: false
 

**Constraints:**

 - $1 \le$ `s.length`, `t.length` $\le 5 \times 10^4$
 - `s` and `t` consist of lowercase English letters.
 

**Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?


## 思路

字母异位词，等价于，两个字符串中字符出现的种类和次数均相等

利用数组 `cnt` 来实现一个简单的哈希表，将字符 a 到 z 映射为数组的下标 0 到 25 ，数组的元素值表示相应字符在字符串 `s` 和 `t` 里出现的次数的差值

若 `cnt` 所有元素值均为 0，字符串 `s` 和 `t` 是字母异位词

代码实现：

```cpp
bool isAnagram(string s, string t) {
    vector<int> cnt(26, 0);     // 计算 s 和 t 各个字母出现次数的差值
    for (auto c : s)
        cnt[c - 'a']++;         // 记录字符串 s 中字符出现的次数
    for (auto c : t)
        cnt[c - 'a']--;         // 记录字符串 t 中字符出现次数的相反数
    for (int i = 0; i < 26; i++)
        if (cnt[i] != 0)        // cnt 元素不为 0 ，则字符串 s 和 t 中的字符不同，不是字母异位词
            return 0;
    return 1;                   // 所有字符出现次数相同，是 s 和 t 是字母异位词
}
```

时间复杂度：$O(n)$

空间复杂度：$O(1)$，常量大小的辅助数组

> 这一题使用数组来实现哈希法，是因为题目限制了只有 26 个小写字母，哈希值比较集中