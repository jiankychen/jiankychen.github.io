---
title: LeetCode 18. 四数之和
tags:
  - 数组
  - 双指针
categories:
  - LeetCode
cover: false
abbrlink: bb4f52a2
date: 2022-05-07 15:43:27
---

[LeetCode 18. 4 Sum](https://leetcode-cn.com/problems/4sum/)

Given an array `nums` of `n` integers, return *an array of all the **unique** quadruplets* `[nums[a], nums[b], nums[c], nums[d]]` such that:

 - `0 <= a, b, c, d < n`
 - `a`, `b`, `c`, and `d` are **distinct**.
 - `nums[a] + nums[b] + nums[c] + nums[d] == target`
 - You may return the answer in **any order**.

 

**Example 1:**

    Input: nums = [1,0,-1,0,-2,2], target = 0
    Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]

**Example 2:**

    Input: nums = [2,2,2,2,2], target = 8
    Output: [[2,2,2,2]]
 

**Constraints:**

 - $1 \le$ `nums.length` $\le 200$
 - $- 10^9 \le$ `nums[i]` $\le 10^9$
 - $- 10^9 \le$ `target` $\le 10^9$


## 思路

与 [LeetCode 15. 三数之和](https://jiankychen.github.io/posts/1b6e2437/) 基本一致

这里采用 排序 + 双指针 解法

## Method: 排序 + 双指针

解题思路：

1. 对数组 `nums` 排序

2. 采用两个 `for` 循环分别遍历 `i` 和 `j` ，其中，`i` 从 `0` 开始遍历，`j` 从 `i + 1` 开始遍历（注意，要分别对 `nums[i]` 和 `nums[j]` 进行去重）

3. 采用双指针法在 `[j + 1, nums.size() - 1]` 区间内查找所有能与 `nums[i]` , `nums[j]` 组成四元组的元素 `nums[left]` 和 `nums[right]` ，其中，指针 `left` 从 `j + 1` 位置开始向右遍历，指针 `right` 从 `nums.size() - 1` 位置开始向左遍历

具体步骤可参考 [LeetCode 15. 三数之和](https://jiankychen.github.io/posts/1b6e2437/) 中的 Method 2

代码实现：

```cpp
vector<vector<int>> fourSum(vector<int>& nums, int target) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> res;
    for (int i = 0; i < nums.size(); i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue; // 对 nums[i] 去重
        for (int j = i + 1; j < nums.size(); j++) {
            if (j > i + 1 && nums[j] == nums[j - 1]) continue; // 对 nums[j] 去重
            int left = j + 1, right = nums.size() - 1;
            int temp1 = nums[i] + nums[j]; // 直接计算 nums[i] + nums[j] + nums[left] + nums[right] 会溢出
            while (left < right) {
                int temp2 = nums[left] + nums[right];
                if (temp2 < target - temp1) left++;
                else if (temp2 > target - temp1) right--;
                else {
                    res.push_back({nums[i], nums[j], nums[left], nums[right]});
                    while (left < right && nums[left] == nums[++left]); // 对 nums[left] 去重
                    while (left < right && nums[right] == nums[--right]); // 对 nums[right] 去重
                }
            }
        }
    }
    return res;
}
```

其中，需要注意 `nums[i] + nums[j] + nums[left] + nums[right]` 会溢出，因此，不能直接将 `nums[i] + nums[j] + nums[left] + nums[right]` 与 `target` 比较，可比较 `nums[left] + nums[right]` 与 `target - nums[i] - nums[j]`

时间复杂度：$O(n^3)$
 - 遍历 `i` 和 `j` ： $O(n^2)$
 - 双指针查找： $O(n)$

空间复杂度：$O(\log{n})$ ，这里不考虑存储结果的数组，仅考虑排序所需栈空间

参考：[代码随想录：四数之和](https://www.programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html)