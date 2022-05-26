---
title: LeetCode 557. 反转字符串中的单词 III
tags:
  - 双指针
  - 字符串
categories:
  - LeetCode
cover: false
abbrlink: e1635286
date: 2022-03-15 17:00:02
---

[LeetCode 557](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)

Given a string s, reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.

 

**Example 1:**

    Input: s = "Let's take LeetCode contest"
    Output: "s'teL ekat edoCteeL tsetnoc"


**Example 2:**

    Input: s = "God Ding"
    Output: "doG gniD"



## Method 1: 双指针（原地解法）
当找到一个单词的时候，我们交换字符串第一个字符与倒数第一个字符，随后交换第二个字符与倒数第二个字符……如此反复，就可以在原空间上翻转单词。

具体流程如下：
1. 指针 `left` 指向第一个字符或空字符前一个字符（双指针中的左端指针）
2. 指针 `search` 向右搜索
3. 当 `search` 搜索到空字符或尾后时，将 `search - 1` 赋给双指针中的右端指针 `right` 
4. 将 `left` 和 `right` 之间的所有字符反转

重复上述操作，直到所有字符均已被反转

```cpp
string reverseWords(string s) {
    int search = 0;
    auto n = s.size();
    while (search < n)
    {
        int left = search;                       // left指向第一个字符或空字符后一个字符
        while (s[search] != ' ' && search < n)   // 向右搜索，直到遇到空字符或者最后一个字符
        {
            search++;
        };
        int right = search - 1;                  // right指向空字符前一个位置，即，单词最后一个字符所在位置
        while (left < right)                     // 反转单词
        {
            swap(s[left], s[right]);
            left++;
            right--;
        };
        search++;                                // 继续搜索
    };
    return s;
}
```

时间复杂度：$O(N)$ ，字符串中的每个字符要么在 $O(1)$ 的时间内被交换到相应的位置，要么因为是空格而保持不动

空间复杂度：$O(1)$ ，因为不需要开辟额外的数组


## Method 2: 双指针（使用额外空间）
开辟一个新字符串。从头到尾遍历原字符串，直到找到空格为止，此时找到了一个单词并得到单词的起止位置。随后，将该单词逆序放到新字符串当中。如此循环多次，直到遍历完原字符串。
```cpp
string reverseWords(string s) {
    string ret;
    int length = s.length();
    int i = 0;
    while (i < length) {
        int start = i;
        while (i < length && s[i] != ' ') {
            i++;
        }
        for (int p = start; p < i; p++) {
            ret.push_back(s[start + i - 1 - p]);
        }
        while (i < length && s[i] == ' ') {
            i++;
            ret.push_back(' ');
        }
    }
    return ret;
}
```

时间复杂度：$O(N)$ ，其中 $N$ 为字符串的长度。原字符串中的每个字符都会在 $O(1)$ 的时间内放入新字符串中

空间复杂度：$O(N)$ ，开辟了与原字符串等大的空间