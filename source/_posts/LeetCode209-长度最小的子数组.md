---
title: LeetCode 209. 长度最小的子数组
date: 2022-04-24 21:03:19
tags:
 - 数组
 - 滑动窗口
categories:
 - LeetCode
cover: false
---

[LeetCode 209](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

Given an array of **positive integers** `nums` and a positive integer `target`, return the **minimal length** of a **contiguous subarray** `[nums[l], nums[l+1], ..., nums[r-1], nums[r]]` of which the sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

 

**Example 1:**

    Input: target = 7, nums = [2,3,1,2,4,3]
    Output: 2
    Explanation: The subarray [4,3] has the minimal length under the problem constraint.


**Example 2:**

    Input: target = 4, nums = [1,4,4]
    Output: 1


**Example 3:**

    Input: target = 11, nums = [1,1,1,1,1,1,1,1]
    Output: 0
 

**Constraints:**

 - 1 <= `target` <= 10^9^
 - 1 <= `nums.length` <= 10^5^
 - 1 <= `nums[i]` <= 10^5^
 

Follow up: If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.


## Method 1: 暴力法

思路：

1. 对每个下标 `i` ，找出 以 `i` 为起点、元素和大于等于 `target` 的子数组的最小长度

2. 从所有的 `i` 中，找到长度最小值

时间复杂度：$O(n^2)$，其中 $n$ 为数组长度

空间复杂度：$O(n)$


## Method 2: 前缀和 + 二分查找

思路：

1. 计算数组 `nums` 的前缀和，存入新数组 `sum` ，其中，`sum[i]` 表示 `nums[0]` 到 `nums[i - 1]` 的元素和

2. 初始化所求长度为 `ans = INT32_MAX`

3. 对每个下标 `i`
    - 利用二分查找算法，找出 `sum` 数组中大于或等于 `sum[i - 1] + target` 的最小下标 `bound`
    - 更新满足条件的子数组的最小长度为 `ans = min(ans, bound - (i - 1))`

[长度最小的子数组：前缀和 + 二分查找](https://leetcode-cn.com/problems/minimum-size-subarray-sum/solution/chang-du-zui-xiao-de-zi-shu-zu-by-leetcode-solutio/)

时间复杂度：$O(n \log{n})$，其中，遍历 `i` 的时间复杂度为 $O(n)$，对每个 `i` 进行二分查找的时间复杂度为 $(\log{n})$

空间复杂度：$O(n)$，额外创建了一个前缀和数组


## Method 3: 滑动窗口

解题思路：

1. 定义指针 `left` 和 `right` 分别表示滑动窗口的左端点和右端点，初始化为 `0`

2. 初始化所求长度为 `res = INT32_MAX`

3. 迭代，窗口右端点 `right` 向右移动，直到 `right == nums.size()`
    - 更新窗口内所有元素之和 `sum += nums[right]`
    - 如果 `sum >= target` ，左端点 `left` 不断向右移（收缩窗口），直到 `sum < target` 。在此过程中，须不断更新 `sum` 和窗口长度 `sublength` ，并记录满足条件的最小长度 `res = (res < sublength) ? res : sublength`

4. 判断 `res` 是否为初值 `INT32_MAX` ：若是，则返回 `0` （不存在满足条件的子数组）；否则，返回 `res`


代码实现：

```cpp
int minSubArrayLen(int target, vector<int>& nums) { // 滑动窗口法
    int res = INT32_MAX;    // 所求的最小长度
    int sum = 0;            // 窗口所有元素值之和
    int sublength = 0;      // 窗口长度
    int left = 0;           // 窗口左端
    for (int right = 0; right < nums.size(); right++) { // 遍历窗口右端
        sum += nums[right];
        while (sum >= target) { // 窗口左端点向右移动，直到窗口所有元素值之和小于 target
            sublength = right - left + 1;   // 更新窗口长度
            res = res < sublength ? res : sublength;    // 更新满足条件的最小长度
            sum -= nums[left++];            // 窗口左端向右移动
        }
    }
    return res == INT32_MAX ? 0 : res;      // 若不存在满足条件的子数组，返回 0
}
```

时间复杂度：$O(n)$，指针 `left` 和 `right` 最多各移动 $n$ 次，$n$ 为数组长度

空间复杂度：$O(1)$