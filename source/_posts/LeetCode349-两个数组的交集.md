---
title: LeetCode 349. 两个数组的交集
tags:
  - 哈希表
  - 数组
categories:
  - LeetCode
cover: false
abbrlink: c10e195a
date: 2022-05-03 11:35:06
---

[LeetCode 349. Intersection of Two Arrays](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

Given two integer arrays `nums1` and `nums2`, return **an array of their intersection**. Each element in the result must be `unique` and you may return the result in **any order**.

 

**Example 1:**

    Input: nums1 = [1,2,2,1], nums2 = [2,2]
    Output: [2]


**Example 2:**

    Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
    Output: [9,4]
    Explanation: [4,9] is also accepted.
 

**Constraints:**

 - $1 \le$ `nums1.length`, `nums2.length` $\le 1000$
 - $0 \le$ `nums1[i]`, `nums2[i]` $\le 1000$


## 思路

由于数组 `nums1` 和 `nums2` 的元素值可能比较分散，使用数组来实现哈希法会造成空间的浪费，故而可以采用 `set`

注意到题目要求输出的每个元素必须唯一，且不考虑输出结果的顺序，故而选用 `unordered_set`

> 注：相比于数组而言，直接使用 set 不仅占用空间比数组大，而且速度慢。因此，在 数组元素大小有限 且 哈希值比较集中 时，应尽量用 数组 ，而如果哈希值比较少、特别分散、跨度非常大，则考虑用 set

## unordered_set

解题步骤：

1. 定义 `unordered_set` 容器 `record` 和 `ans` ，前者存放第一个数组 `nums1` 的元素，后者记录两数组的交集

2. 遍历数组 `nums2` ：若 `nums2` 数组的元素 `a` 能够在 `record` 中找到（即，`record.find(a) != record.end()` ），则说明 `a` 是两数组的公共元素，将其添加到 `ans` 中

代码实现：

```cpp
vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
    unordered_set<int> record(nums1.begin(), nums1.end());  // 将 nums1 拷贝到 unordered_set 容器
    unordered_set<int> ans;             // 存放结果（两数组的交集）
    for (auto a : nums2)
        if (record.find(a) != record.end())   // nums2 的元素 a 在 nums1 中出现过
            ans.insert(a);              // 将 a 添加到结果
    return vector<int>(ans.begin(), ans.end());   // 返回（先拷贝到 vector ，因为返回值的类型是 vector<int>）
}
```