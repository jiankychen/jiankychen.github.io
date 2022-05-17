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

[LeetCode 344. Reverse String](https://leetcode-cn.com/problems/reverse-string/)

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array **in-place** with `O(1)` extra memory.

 

**Example 1:**

    Input: s = ["h","e","l","l","o"]
    Output: ["o","l","l","e","h"]


**Example 2:**

    Input: s = ["H","a","n","n","a","h"]
    Output: ["h","a","n","n","a","H"]

**Constraints:**

 - $1 \le$ `s.length` $\le 10^5$
 - `s[i]` is a printable ascii character.

## Method: 双指针

解题步骤：

1. 定义 `left` 指向字符数组首元素，`right`指向字符数组尾元素
2. 当 `left < right` 时，执行循环：交换 `s[left]` 和 `s[right]` ，`left` 指针右移一位；`right` 指针左移一位

代码如下：

```cpp
void reverseString(vector<char>& s) {
    int left = 0, right = s.size() - 1; // 双指针
    while (left < right)
        swap(s[left++], s[right--]);    // 交换，并移动指针
}
```

时间复杂度：$O(N)$，其中 $N$ 为字符数组的长度，一共执行了 $N/2$ 次的交换

空间复杂度：$O(1)$


如果库函数仅仅是解题过程中的一小部分，并且你已经很清楚这个库函数的内部实现原理的话，可以考虑使用库函数，例如这里使用的 `swap` 函数

`swap` 函数具有两种实现方式，以交换 `s[left]` 和 `s[right]` 为例：

1. 直接交换数值

        int temp = s[left]; // 中间量
        s[left] = s[right];
        s[right] = temp;

2. 位运算（异或）

        s[left] ^= s[right]; // 中间量
        s[right] ^= s[left]; // 将中间量与 s[right] 进行异或运算， = 号右边得到的是原始的 s[left]
        s[left] ^= s[right]; // 将中间量与 s[right] （即原始的 s[left] ） 进行异或运算， = 号右边得到的是原始的 s[right]