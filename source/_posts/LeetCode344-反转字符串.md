---
title: LeetCode 344. 反转字符串
tags:
  - 双指针
  - 字符串
categories:
  - LeetCode
cover: false
abbrlink: d9baf118
date: 2022-03-15 16:59:29
---

[LeetCode 344](https://leetcode-cn.com/problems/reverse-string/)

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array **in-place** with `O(1)` extra memory.

 

**Example 1:**

    Input: s = ["h","e","l","l","o"]
    Output: ["o","l","l","e","h"]


**Example 2:**

    Input: s = ["H","a","n","n","a","h"]
    Output: ["h","a","n","n","a","H"]



## Method: 双指针

1. 将`left`指向字符数组首元素，`right`指向字符数组尾元素。
2. 当`left < right`：交换`s[left]`和`s[right]`；`left`指针右移一位；`right`指针左移一位。
3. 当`left >= right`，反转结束，返回字符数组即可。

程序如下：
```cpp
void reverseString(vector<char>& s) {
    int left = 0, right = s.size() - 1;
    while (left < right)
    {
        char temp = s[right];
        s[right] = s[left];
        s[left] = temp;
        left++;
        right--;
    };
}
```



时间复杂度：$O(N)$，其中 `N` 为字符数组的长度。一共执行了 `N/2` 次的交换

空间复杂度：$O(1)$。只使用了常数空间来存放若干变量