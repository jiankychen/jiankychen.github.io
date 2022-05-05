---
title: LeetCode 34. 在排序数组中查找元素的第一个和最后一个位置
tags:
  - 数组
  - 二分查找
categories:
  - LeetCode
cover: false
abbrlink: a56851c5
date: 2022-04-19 13:37:20
---

[LeetCode 34](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

Given an array of integers `nums` sorted in `non-decreasing order`, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

    Input: nums = [5,7,7,8,8,10], target = 8
    Output: [3,4]

**Example 2:**

    Input: nums = [5,7,7,8,8,10], target = 6
    Output: [-1,-1]


**Example 3:**

    Input: nums = [], target = 0
    Output: [-1,-1]

**Constraints:**

 - $0\le $ `nums.length` $\le 10^5$
 - $-10^9 \le$ `nums[i]` $\le 10^9$
 - `nums` is a non-decreasing array.
 - $- 10^9 \le $ `target` $\le 10^9$


## Method 1: while (left < right)
```cpp
// 寻找元素的第一个位置（找出第一个大于等于 target 的元素）
int LowerBoundSearch(vector<int> nums, int target) {
    int l = 0, r = nums.size() - 1;
    while (l < r) { // 注意：循环结束时 l = r
        int mid = l + ((r - l) >> 1);
        if (nums[mid] >= target)            // lower bound 一定在区间 [l, mid]
            r = mid;
        else if (nums[mid] < target)        // lower bound 一定在区间 (mid, r]
            l = mid + 1;
    }
    if (nums[l] == target)
        return l;
    else
        return -1;
}

// 寻找元素的最后一个位置（找出最后一个小于等于 target 的元素）
int UpperBoundSearch(vector<int> nums, int target) {
    int l = 0, r = nums.size() - 1;
    while (l < r) {
        int mid = l + ((r - l + 1) >> 1);   // 避免陷入死循环，mid 向上取整
        if (nums[mid] <= target)            // upper bound 一定在区间 [mid, r]
            l = mid;
        else if (nums[mid] > target)        // upper bound 一定在区间 [l, mid)
            r = mid - 1;
    }
    if (nums[r] == target)
        return r;
    else
        return -1;
}


vector<int> searchRange(vector<int>& nums, int target) {
    int n = nums.size();
    if (n == 0) {   // 空数组
        vector<int> ans = {-1,-1};
        return ans;
    }
    int lb = LowerBoundSearch(nums, target);
    int ub = UpperBoundSearch(nums, target);
    vector<int> ans = {lb,ub};
    return ans;
}
```


## Method 2: while (left <= right)
```cpp
// 寻找元素的第一个位置
int LowerBoundSearch(vector<int> nums, int target) {
    int l = 0, r = nums.size() - 1;
    while (l <= r) { // 注意：循环结束时 l = r + 1
        int mid = l + ((r - l) >> 1);
        if (nums[mid] >= target)
            r = mid - 1;             // 注意：循环结束时， l 刚好就是 lower bound
        else if (nums[mid] < target)
            l = mid + 1;
    }
    if (nums[l] == target)
        return l;
    else
        return -1;
}

// 寻找元素的最后一个位置
int UpperBoundSearch(vector<int> nums, int target) {
    int l = 0, r = nums.size() - 1;
    while (l <= r) { // 注意：循环结束时 l = r + 1
        int mid = l + ((r - l) >> 1);
        if (nums[mid] <= target)
            l = mid + 1;
        else if (nums[mid] > target)
            r = mid - 1;            // 注意：循环结束时， r 刚好是 upper bound
    }
    if (nums[r] == target)
        return r;
    else
        return -1;
}
```