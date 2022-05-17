---
title: LeetCode 459. 重复的子字符串
tags:
  - 字符串
  - KMP
categories:
  - LeetCode
cover: false
abbrlink: e28abd31
date: 2022-05-17 20:37:43
---

[LeetCode 459. Repeated Substring Pattern](https://leetcode.cn/problems/repeated-substring-pattern/)

Given a string `s`, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

 

**Example 1:**

    Input: s = "abab"
    Output: true
    Explanation: It is the substring "ab" twice.


**Example 2:**

    Input: s = "aba"
    Output: false


**Example 3:**

    Input: s = "abcabcabcabc"
    Output: true
    Explanation: It is the substring "abc" four times or the substring "abcabc" twice.
 

**Constraints:**

 - $1 \le$ `s.length` $\le 10^4$
 - `s` consists of lowercase English letters.


## 思路

检查字符串是否由重复的子串组成，即，验证字符串 `s` 的周期性

可以通过构造 next 数组，获取字符串 `s` 的周期信息

next 数组最后一位元素记录了整个字符串的 最长相同前后缀 的长度，记作 `next[s.size() - 1]` 。若字符串具有周期性，`next[s.size() - 1]` 就应该是周期的整数倍，且应该是 *字符串的总周期数 - 1* 倍。换而言之，`s.size() - next[s.size() - 1]` 就是周期的长度

有关 next 数组及其构造，可参阅 [KMP 算法](https://jiankychen.github.io/posts/36b55f59/)

以字符串 "abcabcabc" 为例，其 next 数组为：

| 下标 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 字符串 | a | b | c | a | b | c | a | b | c |
| next 数组 | 0 | 0 | 0 | 1 | 2 | 3 | 4 | 5 | 6

其周期长度 = 字符串总长度 - next 数组最后一位元素值 = 9 - 6 = 3

因此，可以作出以下结论：若 **`next[s.size() - 1]` 非零** 且 **字符串总长度能够被周期长度整除** ，字符串具有周期性，即，可以由子串重复多次而构成

有关该方法正确性的理论证明，可参考 [力扣官方题解：重复的子字符串（方法三）](https://leetcode.cn/problems/repeated-substring-pattern/solution/zhong-fu-de-zi-zi-fu-chuan-by-leetcode-solution/)

## Method：KMP 算法

解题流程：

1. 构造 next 数组
2. 找到 next 数组最后一位元素，即，`next[s.size() - 1]`
3. 判断 `next[s.size() - 1]` 是否为 0
    - 若 `next[s.size() - 1] == 0` ，字符串不存在周期，`return false`
    - 若 `next[s.size() - 1] != 0` ，判断 `s.size()` 是否能被 `s.size() - next[s.size() - 1]` 整除
      - 若能够被整除，字符串具有周期性，`return true`
      - 若不能被整除，`return false`

代码实现：

```cpp
// 构造 next 数组
void getNext(vector<int> &next, string s) { // 注意，要对 next 加引用符 &
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

bool repeatedSubstringPattern(string s) {
    vector<int> next(s.size(), 0);
    getNext(next, s);
    // for (auto c : next)  // 打印 next 数组
    //     cout << c << " ";
    // cout << endl;
    int temp = next[s.size() - 1];
    if (temp != 0 && s.size() % (s.size() - temp) == 0)
        return true;
    return false;
}
```

时间复杂度：$O(n)$，其中，$n$ 是字符串 `s` 的长度

空间复杂度：$O(n)$

> 可以通过打印 next 数组来检查 next 数组的构造是否正确，并帮助理解做题思路