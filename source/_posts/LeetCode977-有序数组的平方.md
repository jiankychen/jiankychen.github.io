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
找出非递减数组`nums`中正负整数的分界线`pos`，假定`nums[pos] <= 0`，当我们将数组`nums`中的数平方后，那么`nums[0]`到`nums[pos]`单调递减，`nums[pos+1]`到`nums[n−1]`单调递增。

令`i`, `j`分别指向`pos`, `pos + 1`，分以下四种情况进行操作：
1. `pos`是最后一个元素，即，数组`nums`全是非正数，将数组每个元素平方然后按相反顺序排列；
2. `pos + 1`是第一个元素，即数组`nums`全是正数，将数组每个元素平方然后按原顺序排列；
3. `nums[i] * nums[i] <= nums[j] * nums[j]`，先放`nums[i] * nums[i]`；
4. `nums[i] * nums[i] > nums[j] * nums[j]`，先放`nums[j] * nums[j]`；

程序实现：

```cpp
vector<int> sortedSquares(vector<int>& nums) {
    int mid = -1; // 找到最大的负数或者零所在的位置，即，正负数分界线
    for (int i = 0; i < nums.size(); i++)
    {
        if (nums[i] < 0)
            mid = i;
        else
            break;
    };

    vector<int> ans;
    int i = mid, j = mid + 1;
    while ((i >= 0) || (j < nums.size()))
    {
        if (i < 0)                  // i 超出nums左边界
        {
            ans.push_back(nums[j] * nums[j]);
            j++;
        }
        else if (j == nums.size())  // j 超出nums右边界
        {
            ans.push_back(nums[i] * nums[i]);
            i--;
        }
        else if (nums[i] * nums[i] < nums[j] * nums[j])
        {
            ans.push_back(nums[i] * nums[i]);
            i--;
        }
        else
        {
            ans.push_back(nums[j] * nums[j]);
            j++;
        }
    };
    return ans;
}
```


## Method 2: 双指针
使用两个指针分别指向位置`0`和`n−1`，每次比较两个指针对应的数，选择较大的那个逆序放入答案并移动指针

```cpp
vector<int> sortedSquares(vector<int>& nums) {
    int n = nums.size();
    vector<int> ans(n);     // 注意这里要初始化
    for (int i = 0, j = n - 1, pos = n - 1; i <= j;) {
        if (nums[i] * nums[i] > nums[j] * nums[j]) {
            ans[pos] = nums[i] * nums[i];
            ++i;
        }
        else {
            ans[pos] = nums[j] * nums[j];
            --j;
        }
        --pos;
    }
    return ans;
}
```