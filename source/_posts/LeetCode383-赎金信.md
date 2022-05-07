---
title: LeetCode 383. 赎金信
tags:
  - 哈希表
  - 字符串
categories:
  - LeetCode
cover: false
abbrlink: 579713f1
date: 2022-05-06 20:14:15
---

[LeetCode 383. Ransom Note](https://leetcode-cn.com/problems/ransom-note/)

Given two strings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed from `magazine` and `false` otherwise.

Each letter in `magazine` can only be used once in `ransomNote`.

**Example 1:**

    Input: ransomNote = "a", magazine = "b"
    Output: false


**Example 2:**

    Input: ransomNote = "aa", magazine = "ab"
    Output: false


**Example 3:**

    Input: ransomNote = "aa", magazine = "aab"
    Output: true
 

**Constraints:**

 - $1 \le$ `ransomNote.length`, `magazine.length` $\le 10^5$
 - `ransomNote` and `magazine` consist of **lowercase** English letters.

## Method: 哈希

由于字符串中只有小写字母，可以考虑用 数组 实现哈希法

解题步骤如下：

1. 定义数组 `count` ，数组元素的下标表示字符相当于 `a` 的距离，元素值表示字符在字符串 `magazine` 中出现的次数

2. 依据数组 `count` 判断 `ransomNote` 的每个字符是否都在字符串 `magazine` 内
     - 由于 `magazine` 中每个字符只能在 `ransomNote` 中用一次，在遍历字符串 `ransomNote` 中的每个字符时，需要维护字符所对应的 `count` 元素值，即，“用一次就会少一次”

代码实现：

```cpp
bool canConstruct(string ransomNote, string magazine) {
    vector<int> count(26, 0); // record the letters and their frequency in string magazine
    for (auto c : magazine)
        count[c - 'a']++;
    for (auto c : ransomNote) {
        if (count[c - 'a'])
            count[c - 'a']--; // since each letter in string magazine can only be used once
        else
            return false;     // c cannot be found in string magazine
    }
    return true;
}
```

时间复杂度：$O(n)$

空间复杂度：$O(1)$

> 由于 `ransomNote` 和 `magazine` 都由字母组成，`key` 分布较为集中。与 set 和 map 相比，采用数组更为省时