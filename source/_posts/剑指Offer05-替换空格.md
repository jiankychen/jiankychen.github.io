---
title: 剑指Offer 05. 替换空格
date: 2022-03-27 15:15:53
tags:
 - 字符串
categories:
 - LeetCode
cover: false
---

请实现一个函数，把字符串 `s` 中的每个空格替换成 "%20"

示例 1：

    输入：s = "We are happy."
    输出："We%20are%20happy."

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

时间复杂度：$O(N)$，其中，$N$ 为字符串的长度，遍历赋值执行 $N$ 次循环

空间复杂度：$O(N)$，新建了一个字符串


## Method 2: 原地修改（双指针）

先统计整个字符串中的空格数目，然后把字符串大小扩充为替换后的大小，用 双指针 方法 **从右向左** 移元素

> 很多数组/字符串填充类的问题，都可以先预先扩充数组的大小，然后再从右向左进行操作。 这么做有两个好处：1. 不用申请新数组；2. 从右向左填充元素，避免了从前向后填充元素带来的 “每次添加元素都要将添加元素之后的所有元素向后移动” 。

算法流程：
1. 初始化：空格数量 `count` ，字符串 `s` 的长度 `len`

2. 统计空格数量：遍历 `s` ，遇空格则 `count++`

3. 修改 `s` 长度：将所有空格都替换成 "%20" 后的字符串长度应为 `len + 2 * count`

4. 从右向左遍历修改：`i` 指向原字符串尾部元素， `j` 指向新字符串尾部元素；当 `i = j` 时（代表左边已没有空格）结束循环

    - 当 `s[i]` 不为空格时：执行 `s[j] = s[i]`

    - 当 `s[i]` 为空格时：将字符串闭区间 `[j-2, j]` 的元素修改为 "%20" ；由于修改了 `3` 个元素，因此需要 `j -= 2`

5. 返回已修改的字符串 `s`

```cpp
string replaceSpace(string s) {
    int count = 0, len = s.size();
    // 统计空格数量
    for (auto c : s)
        if (c == ' ') count++;
    // 修改 s 长度
    int newLen = len + 2 * count;
    s.resize(newLen);
    // 从右向左遍历修改
    for(int i = len - 1, j = newLen - 1; i < j; i--, j--) {
        if (s[i] != ' ') s[j] = s[i];
        else {
            s[j - 2] = '%';
            s[j - 1] = '2';
            s[j] = '0';
            j -= 2;         // 因为循环体结束后会进行 j--，故而这里是 j-=2
        }
    }
    return s;
}
```

时间复杂度：$O(N)$，遍历统计、遍历修改皆使用 $O(N)$ 时间

空间复杂度：$O(1)$，由于是原地扩展 `s` 长度，因此使用 $O(1)$ 额外空间