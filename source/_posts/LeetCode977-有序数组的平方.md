---
title: LeetCode 977. 有序数组的平方
date: 2022-03-14 19:54:12
tags:
 - 数组
 - 双指针
categories:
 - LeetCode
cover: false
---

[LeetCode 977](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

Given an integer array nums sorted in **non-decreasing** order, return an array of ***the squares of each number*** sorted in **non-decreasing** order.

 

Example 1:

    Input: nums = [-4,-1,0,3,10]
    Output: [0,1,9,16,100]
    Explanation: After squaring, the array becomes [16,1,0,9,100].
    After sorting, it becomes [0,1,9,16,100].


Example 2:

    Input: nums = [-7,-3,2,3,11]
    Output: [4,9,9,49,121]



## Method 1: 双指针

解题思路：

1. 找出非递减数组 `nums` 中正负整数的分界线 `pos` ，假定 `nums[pos] <= 0` 。当我们将数组 `nums` 中的数平方后，第 `0` 位到第 `pos` 位将单调递减，第 `pos + 1` 位到第 `n − 1` 位将单调递增

2. 创建 `ans` 数组，令 指针 `i` 和 `j`分别指向`pos` 和 `pos + 1`，分以下情况进行操作（类似于 **归并** 操作）：
    - 如果 `i < 0` ，或者，`j < nums.size()` 且 `nums[i] * nums[i] >= nums[j] * nums[j]` ，则填充 `nums[j] * nums[j]` ，并执行 `j++`
    - 否则，填充 `nums[i] * nums[i]` ，并执行 `i--`

代码实现：

```cpp
vector<int> sortedSquares(vector<int>& nums) {
    // 找到最大的负数或者零所在的位置，即，正负数分界线
    int mid = -1;
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] < 0)
            mid = i;
        else
            break;
    }

    // 将结果填充到 ans 数组中（类似于归并操作）
    vector<int> ans;
    int i = mid, j = mid + 1;
    while (j - i <= nums.size()) {
        // 如果 i 超出边界，或者，j 未超出边界且 nums[i] 的平方大于 nums[j] ，则填充 nums[j] * nums[j]
        if ((i < 0) || (j < nums.size() && nums[i] * nums[i] >= nums[j] * nums[j])) {
            ans.push_back(nums[j] * nums[j]);
            j++;
        }
        // 否则，填充 nums[i] * nums[i]
        else {
            ans.push_back(nums[i] * nums[i]);
            i--;
        }
    }
    return ans;
}
```

时间复杂度：$O(n)$

空间复杂度：$O(n)$，使用了额外的数组



## Method 2: 双指针

使用两个指针分别指向位置 `0` 和 `n − 1`，每次比较两个指针对应的数，选择较大的并按照逆序放入答案数组，然后移动指针

代码实现：

```cpp
vector<int> sortedSquares(vector<int>& nums) {
    int n = nums.size();
    vector<int> ans(n, 0);     // 注意这里要初始化
    for (int i = 0, j = n - 1, k = n - 1; i <= j;) {
        if (nums[i] * nums[i] > nums[j] * nums[j]) {
            ans[k] = nums[i] * nums[i];
            i++;
        }
        else {
            ans[k] = nums[j] * nums[j];
            j--;
        }
        k--;
    }
    return ans;
}
```

时间复杂度：$O(n)$

空间复杂度：$O(n)$，使用了额外的数组

另，由于数组按递增顺序排列，可以直接比较 ` - nums[i]` 和 `nums[j]` 即可，而无须比较 `nums[i] * nums[i]` 和 `nums[j] * nums[j]`
 - 若 `- nums[i] > nums[j]` ，即，`nums[i]` 的绝对值大于 `nums[j]` ，则 `nums[i] * nums[i] > nums[j] * nums[j]` 必然成立
 - 若 `- nums[i] <= nums[j]` ，则 `nums[i] * nums[i] <= nums[j] * nums[j]` 必然成立