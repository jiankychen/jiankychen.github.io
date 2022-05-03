---
title: LeetCode 349. 两个数组的交集
date: 2022-05-03 11:35:06
tags:
 - 哈希表
 - 数组
categories:
 - LeetCode
cover: false
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

 - 1 <= `nums1.length`, `nums2.length` <= 1000
 - 0 <= `nums1[i]`, `nums2[i]` <= 1000


## 思路

由于数组 `nums1` 和 `nums2` 的元素值可能比较分散，使用数组来实现哈希法会造成空间的浪费，故而可以采用 `set`

注意到题目要求输出的每个元素必须唯一，且不考虑输出结果的顺序，故而选用 `unordered_set`

