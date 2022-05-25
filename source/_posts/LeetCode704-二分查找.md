---
title: LeetCode 704. 二分查找
tags:
  - 数组
  - 二分查找
categories:
  - LeetCode
cover: false
abbrlink: 4a298fcf
date: 2022-03-16 22:21:17
---

[LeetCode 704](https://leetcode-cn.com/problems/binary-search/)

Given an array of integers `nums` which is sorted in **ascending order**, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its `index`. Otherwise, return `-1`.

You must write an algorithm with $O(\log{n})$ runtime complexity.

 

**Example 1:**

    Input: nums = [-1,0,3,5,9,12], target = 9
    Output: 4
    Explanation: 9 exists in nums and its index is 4


**Example 2:**

    Input: nums = [-1,0,3,5,9,12], target = 2
    Output: -1
    Explanation: 2 does not exist in nums so return -1


**Constraints:**

 - $1 \le$ `nums.length` $\le 10^4$
 - $- 10^4$ < `nums[i]`, `target` < $10^4$
 - All the integers in `nums` are **unique**.
 - `nums` is sorted in ascending order.


## 思路

有序数组，且数组中无重复元素，考虑用二分法

然而，二分查找涉及的很多的 **边界条件** ，例如，是 `while(left < right)` 还是 `while(left <= right)` 呢，是 `right = middle` 还是 `right = middle - 1` 呢？

> 在二分查找的过程中，要保持不变量，就是在 `while` 寻找中每一次边界的处理都要坚持根据 **区间的定义** 来操作，这就是 **循环不变量** 规则

写二分法，区间的定义一般为两种，**左闭右闭**，即 `[left, right]`，或者 **左闭右开**，即 `[left, right)`


## Method 1
定义 `target` 是在一个在 **左闭右闭** 的区间里，也就是 `[left, right]` ，所以有：

 - `right` 初值赋为 `nums.size() - 1` ，因为 `target` 定义在 `[left, right]` 区间内

 - `while (left <= right)` 循环控制条件使用 `<=` ：因为 `left == right` 是依然可行的

 - `if (nums[middle] > target)` 的执行语句中，`right` 要赋值为 `middle - 1` ：因为当前这个 `nums[middle]` 一定不是 `target`，可以更新区间右端点为 `nums[middle]`

代码实现：
```cpp
int search(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1; // 定义target在左闭右闭的区间里，[left, right]
    while (left <= right) {      // 当left==right，区间[left, right]依然有效，所以用 <=
        int middle = left + ((right - left) / 2);// 防止溢出 等同于(left + right)/2
        if (nums[middle] > target)
            right = middle - 1;  // target 在左区间，所以[left, middle - 1]
        else if (nums[middle] < target)
            left = middle + 1;   // target 在右区间，所以[middle + 1, right]
        else                     // nums[middle] == target
            return middle;       // 数组中找到目标值，直接返回下标
    }
    // 未找到目标值
    return -1;
}
```


## Method 2
如果定义 `target` 在一个在 **左闭右开** 的区间里，也就是 `[left, right)` ：

 - `right` 初值赋为 `nums.size()` ，因为 `target` 定义在 `[left, right)` 区间内

- `while (left < right)` 循环控制条件使用 `<` ：因为 `left == right` 在区间 `[left, right)` 是不可行的
- `if (nums[middle] > target)` 的执行语句中，`right` 更新为 `middle` ：因为搜索区间是 左闭右开 区间，而当前的 `nums[middle]` 不等于 `target` ，所以 `right` 更新为 `middle` 时，下一个搜索区间就不会再去比较 `nums[middle]` （注意，这里不能将 `right` 更新为 `middle - 1` ：若 `right` 赋值为 `middle - 1` ，则排除了 `middle - 1` 的可能性）

```cpp
int search(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size();    // 定义target在左闭右开的区间里，即：[left, right)
    while (left < right) {      // 因为left == right在区间[left, right)是不可行的，所以使用 <
        int middle = left + ((right - left) >> 1); // 等同于left + ((right - left) / 2)
        if (nums[middle] > target)
            right = middle;     // target 在左区间，在[left, middle)中
        else if (nums[middle] < target)
            left = middle + 1;  // target 在右区间，在[middle + 1, right)中
        else                    // nums[middle] == target
            return middle;      // 数组中找到目标值，直接返回下标
    }
    // 未找到目标值
    return -1;
}
```

> 在循环中要坚持根据搜索区间的定义来做边界处理

参考：[代码随想录：704. 二分查找](https://www.programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html#_704-%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE)