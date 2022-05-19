---
title: LeetCode 1047. 删除字符串中的所有相邻重复项
tags:
  - 栈
categories:
  - LeetCode
cover: false
abbrlink: 7fd44fa1
date: 2022-05-19 22:57:23
---


[1047. Remove All Adjacent Duplicates In String](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)


You are given a string `s` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.

We repeatedly make **duplicate removals** on `s` *until we no longer can.*

Return *the final string after all such duplicate removals have been made.* It can be proven that the answer is **unique**.

 

**Example 1:**

    Input: s = "abbaca"
    Output: "ca"
    Explanation: 
    For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".


**Example 2:**

    Input: s = "azxxzy"
    Output: "ay"
 

**Constraints:**

 - $1 \le$ `s.length` $\le 10^5$
 - `s` consists of lowercase English letters.

## 思路

匹配问题 都是 栈 的强项

基本思路：遍历字符串，若当前字符与栈顶元素相等，将栈顶元素移除，否则，将字符压入栈中。遍历完字符串以后，栈内不再含有连续的重复字符，将栈内元素依次弹出、并填充到目标字符串即可

## Method: 栈

由于栈顶元素对应的是字符串的尾端，填充目标字符串时需按从右往左顺序填充

代码实现：

```cpp
string removeDuplicates(string s) {
    // 将字符依次压入栈，若遇连续相同字符，则将栈顶元素移除
    stack<int> stk;
    for (auto c : s) {
        if (!stk.empty() && c == stk.top()) stk.pop();
        else stk.push(c);
    }
    // 将栈内字符添加到新字符串中（最先出栈的放到字符串末尾）
    int size = stk.size();
    string res(size, ' ');
    for (int i = size - 1; i >= 0; i--) {
        res[i] = stk.top();
        stk.pop();
    }
    return res;
}
```

时间复杂度：$O(n)$，其中，$n$ 为字符串长度

空间复杂度：$O(n)$，栈所需空间（这里没有把目标字符串所需空间考虑在内）

> 也可以将栈内元素按从左往右顺序填充到目标字符串，然后再对目标字符串进行翻转

## Method: 字符串

在 C++ 中，由于标准库类型 `string` 本身就提供了类似 入栈 和 出栈 的接口，可直接将目标字符串作为栈


代码实现：

```cpp
string removeDuplicates(string s) {
    string res = "";
    for (auto c : s) { // 从目标字符串尾部移除与 c 相同的字符
        if (!res.empty() && c == res.back())
            res.pop_back();
        else           // 将字符 c 添加到目标字符串尾部
            res.push_back(c);
    }
    return res;
}
```

时间复杂度：$O(n)$，其中，$n$ 为字符串长度

空间复杂度：$O(1)$，没有把目标字符串所需空间考虑在内

参考：
 - [代码随想录](https://www.programmercarl.com/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.html)
 - [力扣官方题解](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/solution/shan-chu-zi-fu-chuan-zhong-de-suo-you-xi-4ohr/)