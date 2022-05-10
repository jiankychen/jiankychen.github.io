---
title: LeetCode 151. 颠倒字符串中的单词
tags:
  - 字符串
  - 双指针
categories:
  - LeetCode
cover: false
abbrlink: 2b58e836
date: 2022-05-10 14:36:11
---

[LeetCode 151. Reverse Words in a String](https://leetcode.cn/problems/reverse-words-in-a-string/)

Given an input string `s`, reverse the order of the **words**.

A word is defined as a sequence of non-space characters. The words in `s` will be separated by at least one space.

**Return a string of the words in reverse order concatenated by a single space.**

Note that `s` may contain leading or trailing spaces or multiple spaces between two words. *The returned string should only have a single space separating the words.* Do not include any extra spaces.

 

Example 1:

    Input: s = "the sky is blue"
    Output: "blue is sky the"


Example 2:

    Input: s = "  hello world  "
    Output: "world hello"
    Explanation: Your reversed string should not contain leading or trailing spaces.


Example 3:

    Input: s = "a good   example"
    Output: "example good a"
    Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
 

Constraints:

 - $1 \le$ `s.length` $\le 10^4$
 - `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
 - There is **at least one** word in `s`.
 

**Follow-up:** If the string data type is mutable in your language, can you solve it **in-place** with `O(1)` extra space?


## 思路

解题思路：

1. 移除字符串中多余的空格，包括字符串首部和尾部的空格以及单词之间的多个空格

2. 翻转整个字符串

3. 逐个翻转每个单词

以 "a good   example" 为例：
 - 移除多余空格："a good example"
 - 翻转字符串："elpmaxe doog a"
 - 翻转单词："example good a"


可参考
 - [LeetCode 27. 移除元素](https://jiankychen.github.io/posts/c425967e/)
 - [LeetCode 344. 反转字符串](https://jiankychen.github.io/posts/d9baf118/)
 - [LeetCode 557. 反转字符串中的单词 III](https://jiankychen.github.io/posts/e1635286/)

## Method: 双指针

算法流程：

1. 去除多余空格
    - 定义快慢指针 `fast` 和 `slow` ，`slow` 指向 `0` ，`fast` 指向从左往右的第一个非空格字符
    - 将 `s[fast]` 赋给 `s[slow]` ，然后移动 `fast` 和 `slow` 指针，重复该过程，特别地，如果 `fast` 遇到连续的多个空格，只需将一个空格赋给 `s[left]`
    - 当 `fast` 移动到字符串的尾后时，`slow` 指向的是原字符串的最后一个字符。如果 `s[slow]` 是空字符，将 `slow` 指针向左移动，直到 `slow` 指向非空字符

2. 调整字符串 `s` 的长度：上一步中的 `slow` 指向最后一个有效字符，故，有效字符串的长度为 `slow + 1`

3. 翻转整个字符串
    - 定义 `reverse` 函数，实现左右指针 `left` 和 `right` 之间所有字符的翻转：交换 `s[left]` 与 `s[right]` ，并将 `left` 向右移动、将 `right` 向左移动

4. 翻转字符串中的每个单词
    - 定义 `start` 和 `end` 指针，`start` 指向单词的起始位置，当 `end` 指向单词后的空格时或者指向字符串的尾后时，调用 `reverse` 函数翻转 `start` 到 `end - 1` 之间的字符

代码实现：

```cpp
// 将 left 到 right 之间的所有字符反转
void reverse(string &s, int left, int right) {
    while (left < right)
        swap(s[left++], s[right--]);
}

// 反转字符串中的单词
string reverseWords(string s) {
    // 双指针，去掉多余空格
    int slow = 0, fast = 0;
    while (fast < s.size() && s[fast] == ' ') fast++; // 跳过字符串首部的空格
    for (; fast < s.size(); fast++) {
        if (s[fast] == ' ' && s[fast - 1] == ' ')     // 跳过重复的空格
            continue;
        s[slow++] = s[fast];          // 移动元素
    }
    slow--;                           // slow 指向元素移动后的末尾字符
    while (s[slow] == ' ') slow--;    // slow 移到最后一个非空格字符的位置（排除字符串尾部的空格）
    s.resize(slow + 1);               // 调整 s 的大小
    // 反转整个字符串
    reverse(s, 0, s.size() - 1);
    // 双指针法，反转每个单词
    int left = 0, right = 0;
    while (right <= s.size()) {
        if (s[right] == ' ' || right == s.size()) { // right 遇到空格或者到达尾后，反转单词
            reverse(s, left, right - 1);
            left = right + 1;
        }
        right++;
    }
    return s;
}
```

时间复杂度：$O(n)$

空间复杂度：$O(1)$