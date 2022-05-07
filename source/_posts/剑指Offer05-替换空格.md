---
title: 剑指Offer 05. 替换空格
tags:
  - 字符串
  - 双指针
categories:
  - LeetCode
cover: false
abbrlink: fff72920
date: 2022-03-27 15:15:53
---

[剑指Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

Please replace each **space** in string `s` with "**%20**".

**Example 1：**

    Input：s = "We are happy."
    Output："We%20are%20happy."

**Constraint：**
 - $0 \le$ `s.length` $\le 10000$

## Method 1: 遍历添加

遍历字符串 `s` 中的每个字符，如果不是空格，直接赋值给新字符串；否则，赋值 “%20” 到新字符串

```cpp
string replaceSpace(string s) {
    int i = 0;
    string ans = "";
    while (i < s.size()) {
        if (isspace(s[i])) ans += "%20"; // 等价于 ans.append("%20")
        else ans += s[i];                // 等价于 ans.push_back(s[i])
        i++;
    }
    return ans;
}
```

其中，`string` 的成员函数：

   - `append()` ：向后加入的是 `string` 类型

   - `push_back()` ：向后加入的是 `char` 类型

时间复杂度：$O(n)$，其中，$n$ 为字符串的长度

空间复杂度：$O(n)$，新建了一个字符串


## Method 2: 原地修改（双指针）

基本思路：先统计整个字符串中的空格数目，然后把字符串大小扩充为替换后的大小，用 双指针 方法 **从右向左** 移元素

解题步骤：

1. 统计空格数量 `count`

2. 修改 `s` 长度：将所有空格都替换成 "%20" 后，新字符串应比原字符串长 `2 * count`

3. 定义指针 `left` 指向原字符串尾部元素， `right` 指向新字符串尾部元素

4. 从右向左遍历修改 `s` ，当 `left == right` 时（代表左边已没有空格）结束循环

    - 当 `s[left]` 不是空格时，执行 `s[right] = s[left]`

    - 当 `s[left]` 是空格时，将区间 `[right - 2, right]` 内的元素修改为 "%20"

代码实现：

```cpp
string replaceSpace(string s) {
    // 统计空格数量
    int count = 0;
    for (auto c : s)
        if (c == ' ') count++;
    // 扩充字符串长度
    int oldSize = s.size(), newSize = oldSize + 2 * count;
    s.resize(newSize);
    // 双指针法
    int left = oldSize - 1, right = newSize - 1;
    while (left < right) {
        if (s[left] == ' ') {
            s[right--] = '0';
            s[right--] = '2';
            s[right--] = '%';
            left--;
        }
        else
            s[right--] = s[left--];
    }
    return s;
}
```

时间复杂度：$O(n)$，统计空格数量、双指针法修改 `s` 的时间复杂度均为 $O(n)$

空间复杂度：$O(1)$，由于是原地扩展 `s` 长度，因此使用 $O(1)$ 额外空间

很多数组/字符串填充类的问题，都可以预先扩充数组的大小，然后再从右向左进行操作

这样做有两个好处：
1. 不用申请新数组，降低了空间复杂度
2. 从右向左填充元素，避免了从左向右填充元素带来的 “每次添加元素都要将该位置右边的元素向后移动” 的问题，既降低了时间复杂度，也简化了算法

参考：[代码随想录](https://www.programmercarl.com/%E5%89%91%E6%8C%87Offer05.%E6%9B%BF%E6%8D%A2%E7%A9%BA%E6%A0%BC.html)