---
title: LeetCode 20. 有效的括号
abbrlink: 2998838f
date: 2022-05-19 16:16:36
tags:
  - 栈
categories:
  - LeetCode
cover: false
---


[LeetCode 20. Valid Parentheses](https://leetcode.cn/problems/valid-parentheses/)


Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

 - Open brackets must be closed by the same type of brackets.
 - Open brackets must be closed in the correct order.
 

**Example 1:**

    Input: s = "()"
    Output: true


**Example 2:**

    Input: s = "()[]{}"
    Output: true


**Example 3:**

    Input: s = "(]"
    Output: false
 

**Constraints:**

 - $1 \le$ `s.length` $\le 10^4$
 - `s` consists of parentheses only `'()[]{}'`.


## 思路

从左往右遍历字符串时，后遇到的左括号需要先匹配，与栈后进先出的特点不谋而合

因此，**括号匹配问题可以使用 栈 来解决**

基本思路：遍历字符串，若遇到左括号，则将其入栈，若遇到右括号，则将对应栈顶左括号出栈

考察 括号不匹配 的三种情况：
 - 存在多余的左括号：在这种情况下，待字符串遍历结束后，栈不为空
 - 存在多余的右括号：字符串未遍历完，栈已经为空
 - 括号没有多余，但是括号的方向不对应：在遍历字符串过程中，遍历到的右括号无法与栈顶左括号匹配

若字符串遍历结束后，栈为空，则说明括号全部匹配


## Method 1: 栈 + map

栈 存储的是 待匹配的左括号

哈希 map 用来存储可以匹配的括号类型，即：哈希表的 key 为右括号，value 为相同类型的左括号，这样查询 2 个括号是否对应只需 $O(1)$ 时间复杂度

具体可参考 [力扣官方题解：有效的括号](https://leetcode.cn/problems/valid-parentheses/solution/you-xiao-de-gua-hao-by-leetcode-solution/) 和 [Krahets：有效的括号](https://leetcode.cn/problems/valid-parentheses/solution/valid-parentheses-fu-zhu-zhan-fa-by-jin407891080/)

## Method 2: 栈

遍历字符串过程中，若遇到左括号，可将对应的右括号压入栈，后续遇到右括号时，只需将其与栈顶元素进行比较即可，若相等，则匹配，否则不匹配

即，栈 存储的是 与左括号对应的右括号

代码实现：

```cpp
bool isValid(string s) {
    if (s.size() % 2) return false; // 括号数为奇数，直接返回 false
    stack<int> stk;
    for (auto c : s) { // 范围 for
        // 当 c 为左括号时，把对应的 右括号 压入栈
        if (c == '(') stk.push(')');
        else if (c == '[') stk.push(']');
        else if (c == '{') stk.push('}');
        // 当栈为空，或者，c 是 右括号 但 不等于栈顶元素，匹配失败
        else if (stk.empty() || c != stk.top())
            return false;
        // c 是 右括号 且 等于栈顶元素，匹配成功，从栈顶移除左括号
        else
            stk.pop();
    }
    // 所有括号均已遍历完成，若栈为空，则匹配成功
    return stk.empty();
}
```

时间复杂度：$O(n)$，遍历字符串

空间复杂度：$O(n)$，栈所需空间


参考：[代码随想录：有效的括号](https://www.programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html#%E9%A2%98%E5%A4%96%E8%AF%9D)