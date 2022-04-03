---
title: LeetCode 35. 搜索插入位置
date: 2022-03-12 20:40:33
tags: 
 - 二分查找
 - 数组
categories:
 - LeetCode
cover: false
---

[LeetCode 35](https://leetcode-cn.com/problems/search-insert-position/)

Given a sorted array of distinct integers and a `target` value, return the `index` if the `target` is found. If not, return the `index` where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

 

Example 1:

    Input: nums = [1,3,5,6], target = 5
    Output: 2


Example 2:

    Input: nums = [1,3,5,6], target = 2
    Output: 1


Example 3:

    Input: nums = [1,3,5,6], target = 7
    Output: 4



## Method: 二分查找
在一个有序数组中找到第一个大于等于 `target` 的下标
```cpp
int searchInsert(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target)
            left = mid + 1;
        else
            right = mid - 1;
    };
    return left;
}
```

二分查找需要注意边界条件，比如：循环结束条件中 `left` 和 `right` 的关系，更新 `left` 和 `right` 位置时要不要加 1 减 1。


[写对二分查找不是套模板并往里面填空，需要仔细分析题意](https://leetcode-cn.com/problems/search-insert-position/solution/te-bie-hao-yong-de-er-fen-cha-fa-fa-mo-ban-python-/)