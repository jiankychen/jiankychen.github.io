---
title: LeetCode 454. 四数相加II
date: 2022-05-04 17:08:58
tags:
 - 哈希表
 - 数组
categories:
 - LeetCode
description:
cover: false
top_img: /img/Earth.jpg
---

[LeetCode 454. 4Sum II](https://leetcode-cn.com/problems/4sum-ii/)

Given four integer arrays `nums1`, `nums2`, `nums3`, and `nums4` all of length `n`, return the number of tuples `(i, j, k, l)` such that:

 - `0 <= i, j, k, l < n`
 - `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`
 

**Example 1:**

    Input: nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
    Output: 2
    Explanation:
    The two tuples are:
    1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
    2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0


**Example 2:**

    Input: nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
    Output: 1
 

**Constraints:**

 - `n == nums1.length`
 - `n == nums2.length`
 - `n == nums3.length`
 - `n == nums4.length`
 - 1 <= `n` <= 200
 - -2^28^ <= `nums1[i]`, `nums2[i]`, `nums3[i]`, `nums4[i]` <= 2^28^

## 思路

统计 `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0` 成立的 `(i, j, k, l)` 的个数，不用考虑是否有重复的元素值相加

可以采用 `unordered_map` 容器记录 任意 `nums1[i]` 与 `nums2[j]` 相加的和，将其作为 `key`，同时记录 该 `key` 值出现的次数，将其作为对应的 `value`

然后遍历 `nums3[k]` 和 `nums4[l]` ，看 `unordered_map` 容器中是否能找到值为 `0 - num3[k] - num4[l]` 的 `key` ，将找到的所有 `key` 对应的 `value` 进行求和，即为所求

## 哈希 map

解题步骤：

1. 定义一个 `unordered_map` 容器，命名为 `map`
2. 遍历数组 `nums1` 的元素`a` 和 数组 `nums2` 的元素 `b`，将 `a + b` 作为哈希 `map` 的 `key` ，将值 `a + b` 出现的次数作为对应的 `value`
3. 定义变量 `count` ，用来记录答案
4. 遍历数组 `nums3` 元素 `c` 和数组 `nums4` 元素 `d` ，如果在哈希 `map` 中找到 `0 - (c + d)` ，把对应的 `value` 累加到 `count`

代码实现：

```cpp
int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
    // 统计 nums1 和 nums2 数组中任意两元素之和
    unordered_map<int, int> map;
    for (auto a : nums1)
        for (auto b : nums2)
            map[a + b]++;
    // 统计 a + b + c + d = 0 的次数
    int count = 0;
    for (auto c : nums3)
        for (auto d : nums4)
            if (map.find(0 - c - d) != map.end())
                count += map[0 - c - d];
    return count;
}
```

时间复杂度：$O(n^2)$
 - 两个 `for` 循环，时间复杂度为 $O(n^2)$
 - 每次哈希查找的时间复杂度为 $O(1)$

空间复杂度：$O(n^2)$，哈希 `map` 所需要的空间

参考：[代码随想录：四数相加II](https://www.programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)