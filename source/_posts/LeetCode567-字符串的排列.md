---
title: LeetCode 567. 字符串的排列
date: 2022-03-22 19:58:19
tags: 
 - 字符串
 - 双指针
categories:
 - LeetCode
cover: false
---

[LeetCode 567](https://leetcode-cn.com/problems/permutation-in-string/)

Given two strings `s1` and `s2`, return `true` *if `s2` contains a permutation of `s1`*, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.
 

Example 1:

    Input: s1 = "ab", s2 = "eidbaooo"
    Output: true
    Explanation: s2 contains one permutation of s1 ("ba").


Example 2:

    Input: s1 = "ab", s2 = "eidboaoo"
    Output: false



## Method 1: 滑动窗口
> 两个字符串经过排序之后相等，可以说明一个字符串是另一个的排列。（时间复杂度高）

> 只有当两个字符串每个字符的个数均相等时，一个字符串才是另一个字符串的排列

记 `s1` 的长度为 `n`，可以遍历 `s2` 中每个长度为 `n` 的子串，判断子串和 `s1` 中每个字符的个数是否相等，若相等则说明该子串是 `s1` 的一个排列

数组 `cnt1` 统计 `s1` 中各个字符的个数，`cnt2` 统计当前遍历的子串中各个字符的个数

由于需要遍历的子串长度均为 `n`，可以使用一个固定长度为 `n`的滑动窗口来维护 `cnt2`：滑动窗口每向右滑动一次，就多统计一次进入窗口的字符，少统计一次离开窗口的字符。然后，判断 `cnt1`​ 是否与 `cnt2` ​相等，若相等则意味着 `s1` 的排列之一是 `s2` 的子串。

```cpp
bool checkInclusion(string s1, string s2) {
    int n1 = s1.size(), n2 = s2.size();
    if (n1 > n2) return false;
    vector<int> cnt1(26),cnt2(26);
    for (int i = 0; i < n1; i++) {  // 统计s1每个字符的出现次数，s2中首个长为n1子串每个字符的出现次数
        ++cnt1[s1[i] - 'a'];
        ++cnt2[s2[i] - 'a'];
    }
    if (cnt1 == cnt2) return true;
    for (int i = n1; i < n2; i++) { // 滑动窗口法（窗长为n1），更新窗口内每个字符的出现次数
        ++cnt2[s2[i] - 'a'];        // 进入窗口的字符
        --cnt2[s2[i- n1] - 'a'];    // 离开窗口的字符
        if (cnt1 == cnt2) return true;
    }
    return false;
}
```

## Method 2: 滑动窗口
注意到每次窗口滑动时，只统计了一进一出两个字符，却比较了整个 `cnt1` 和 `cnt2` 数组。因此可对上述算法进行优化：用一个变量 `diff` 来记录 `cnt1` 与 `cnt2` 的不同值的个数，这样判断 `cnt1` 和 `cnt2` 是否相等就转换成了判断 `diff` 是否为 `0`

为简化上述逻辑，可以只用一个数组 `cnt`，其中 `cnt[x]=cnt2[x]−cnt1[x]`，将 `cnt1[x]` 与 `cnt2[x]` 的比较替换成 `cnt[x]` 与 `0` 的比较

```cpp
bool checkInclusion(string s1, string s2) {
    int n1 = s1.size(), n2 = s2.size();
    if (n1 > n2) return false;
    // 数组cnt记录的是s1字符串与s2窗口中，每个字符出现次数的差异值：若cnt1 cnt2分别记录s1字符串与s2窗口中每个字符出现次数，则cnt=cnt2-cnt1
    vector<int> cnt(26);        // s1 s2仅包含小写字母
    for (int i = 0; i < n1; i++) {
        --cnt[s1[i] - 'a'];
        ++cnt[s2[i] - 'a'];
    }
    int diff = 0;               // 记录cnt非零元素的个数，即，cnt1与cnt2不相等元素的个数
    for (auto c : cnt)
        if (c != 0)
            ++diff;
    if (diff == 0) return true; // diff等于0，即cnt1与cnt2相同，输出true
    for (int i = n1; i < n2; i++) {
        int x = s2[i] - 'a', y = s2[i - n1] - 'a'; // x对应进入窗口的字符，y对应离开窗口的字符
        if (x == y) continue;           // x等于y时，无需修改cnt和diff
        if (cnt[x] == 0) ++diff;        // cnt1[x]原本等于cnt2[x]，新进入的x导致diff加1
        ++cnt[x];
        if (cnt[x] == 0) --diff;        // 进入窗口的x使得cnt1[x]等于cnt2[x]，故diff减1（该if语句是上一条if语句是互斥的）
        if (cnt[y] == 0) ++diff;
        --cnt[y];
        if (cnt[y] == 0) --diff;
        if (diff == 0) return true;
    }
    return false;
}
```

时间复杂度：`O(n + m +∣Σ∣)`，其中 `n` 是字符串 `s1` 的长度，`m` 是字符串 `s2` 的长度，`Σ` 是字符集，这道题中的字符集是小写字母，`∣Σ∣=26`

空间复杂度：`O(∣Σ∣)`


## Method 3: 双指针

方法一的思路：在保证区间长度为 `n` 的情况下，去考察是否存在一个区间使得 `cnt` 的值全为 `0`。反过来，还可以在保证 `cnt` 的值不为正的情况下，去考察是否存在一个区间，其长度恰好为 `n`

初始时，仅统计 `s1` 中的字符，则 `cnt` 的值均不为正，且元素值之和为 `-n`

用指针 `left` 和 `right` 表示考察的区间 `[left,right]`。`right` 每向右移动一次，就统计一次进入区间的字符 `x`。为保证 `cnt` 的值不为正，若此时 `cnt[x] > 0`，则向右移动左指针，减少离开区间的字符的 `cnt` 值直到 `cnt[x] ≤ 0`。

注意到 `[left,right]` 的长度每增加 `1`，`cnt` 的元素值之和就增加 `1`。当 `[left,right]` 的长度恰好为 `n` 时，就意味着 `cnt` 的元素值之和为 `0`。由于 `cnt` 的值不为正，元素值之和为 `0` 就意味着所有元素均为 `0`，这样我们就找到了一个目标子串。

```cpp
bool checkInclusion(string s1, string s2) {
    int n1 = s1.size(), n2 =s2.size();
    if (n1 > n2) return false;
    vector<int> cnt(26);
    for (int i = 0; i < n1; i++)
        --cnt[s1[i] - 'a'];     // cnt的元素值之和为-n1
    int l = 0;
    // 在保证 cnt 的值不为正（所有元素均不为正）的情况下，去考察是否存在一个区间，其长度恰好为 n1
    for (int r = 0; r < n2; r++) {
        int x = s2[r] - 'a';
        ++cnt[x];
        while (cnt[x] > 0) { // x出现次数多，l右移，并更新cnt
            --cnt[s2[l] - 'a'];
            ++l;
        }
        if (r - l + 1 == n1) return true;   // cnt各元素均不为正，且区间[l,r]长度刚好为n1，找到目标
    }
    return false;
    
}
```


时间复杂度：`O(n + m +∣Σ∣)`。创建 `cnt` 需要 `O(∣Σ∣)` 的时间。遍历 `s1` 需要 `O(n)` 的时间。双指针遍历 `s2` 时，由于 `left` 和 `right` 都只会向右移动，故这一部分需要 `O(m)` 的时间。

空间复杂度：`O(∣Σ∣)`。
