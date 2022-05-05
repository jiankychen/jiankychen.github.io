---
title: LeetCode 3. 无重复字符的最长子串
tags:
  - 字符串
  - 双指针
categories:
  - LeetCode
cover: false
abbrlink: 3deb1e70
date: 2022-03-22 19:57:44
---

[LeetCode 3](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

Given a string `s`, find the length of the **longest substring** without repeating characters.

 

**Example 1:**

    Input: s = "abcabcbb"
    Output: 3
    Explanation: The answer is "abc", with the length of 3.


**Example 2:**

    Input: s = "bbbbb"
    Output: 1
    Explanation: The answer is "b", with the length of 1.


**Example 3:**

    Input: s = "pwwkew"
    Output: 3
    Explanation: The answer is "wke", with the length of 3.
    Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.



## Method: 滑动窗口

使用两个指针表示字符串中的某个子串（或窗口）的左右边界，该窗口容纳无重复字符的子串

重复以下循环，直至右指针到达尾后：
1. 判断右指针对应字符是否与窗口内字符重复
2. 若重复，更新 *无重复字符最长子串* 的长度，并找到窗口内重复字符所在位置的下一位，令左指针指向该位置
3. 若不重复，将右指针对应字符当前无重复字符子串（右指针继续右移），继续循环

程序实现：
```cpp
private:
    int isRepeat(string str, int l, int r) { // 判断s[r]是否与s[l]到s[r-1]之间的字符重复，输出重复字符所在位置加1，或输出0
        int i = l;
        while (i <= r - 1) {
            if (str[i] == str[r]) return (i + 1); // 返回i+1而不是i，为了避免s[0]与s[r]相同时输出0
            i++;
        }
        return 0;
    };

public:
    int lengthOfLongestSubstring(string s) {
        int n = s.size(), ans = 0;
        if (n <= 1) ans = n;
        else {
            int left = 0, right = 1;    // left为窗口左端，right为窗口右端
            while (right < n) {
                int tag = isRepeat(s,left,right);
                while (right < n - 1 && !tag) { // 窗口右端右移，直至遇到重复字符
                    right++;
                    tag = isRepeat(s,left,right);
                }
                if (right == n - 1 && ! isRepeat(s,left,right)) right++;
                int temp = right - left;        // 当前无重复字符子串的长度
                if (temp >= ans) ans = temp;    // 更新无重复字符最长子串的长度
                left = tag;                     // 滑动窗口，使left移到与s[right]重复字符的下一位（即s[tag]）
                right++;
            }
        }
        return ans;
    }
};
```

程序中的 `isRepeat` 函数，采用的是遍历的方式来判断重复字符。事实上，可以借助哈希集合（即 C++ 中的 `std::unordered_set` ）来判断。在左指针向右移动的时候，我们从哈希集合中移除一个字符，在右指针向右移动的时候，我们往哈希集合中添加一个字符。

[题解：滑动窗口](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/hua-dong-chuang-kou-by-powcai/)