---
title: LeetCode 1. 两数之和
tags:
  - 哈希表
categories:
  - LeetCode
cover: false
abbrlink: 3da9e18f
date: 2022-05-03 21:06:21
---

[LeetCode 1. Two Sum](https://leetcode-cn.com/problems/two-sum/)

Given an array of integers `nums` and an integer `target`, return **indices of the two numbers such that they add up to `target`**.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

 

**Example 1:**

    Input: nums = [2,7,11,15], target = 9
    Output: [0,1]
    Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].


**Example 2:**

    Input: nums = [3,2,4], target = 6
    Output: [1,2]


**Example 3:**

    Input: nums = [3,3], target = 6
    Output: [0,1]
 

**Constraints:**

 - $2$ <= `nums.length` <= $10^4$
 - $- 10^9$ <= `nums[i]` <= $10^9$
 - $- 10^9$ <= `target` <= $10^9$
 - **Only one valid answer exists.**
 

**Follow-up:** Can you come up with an algorithm that is less than $O(n^2)$ time complexity?



## 思路

解题思路：遍历数组的下标 `i` ，判断 `target - nums[i]` 是否存在，若存在，返回 `i` 以及 数组中值为 `target - nums[i]` 的元素的下标

本题不仅要判读数值是否存在，还要记录其下标，因此采用哈希 map ，其中，元素值 作为 `key` ，元素在数组中的索引下标作为 `value`

## Method: 哈希 map

由于并不需要 `key` 有序，可以选择 `unordered_map`

[std::unoredered_map](http://www.cplusplus.com/reference/unordered_map/unordered_map/?kw=unordered_map)

解题步骤：

1. 定义 `unodered_map` 容器，命名为 `map`

2. 遍历数组下标 `i`
    - 查找 `target - nums[i]` 是否存在于 `map` 当中
    - 若存在，返回下标 `i` 以及 与键值 `target - nums[i]` 对应的 `value`
    - 否则，将 `nums[i]` 以及 `i` 作为键值对添加到 `map`


代码实现：
```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> map;
    for (int i = 0; i < nums.size(); i++) {
        auto temp = map.find(target - nums[i]); // find 返回的是迭代器类型
        if (temp != map.end())                  // 找到 target - nums[i]
            return {temp->second, i};           // 返回 i 以及 temp 的第二个值
                                                // temp->first 是 key ，temp->second 是 value
        map.insert(pair<int, int>(nums[i], i)); // 将 nums[i], i 作为键值对插入到 map
    }
    return {};
}
```

> `unordered_map` 的成员函数 `find` 的返回值是 `iterator` 类型，解引用后的第一个成员为 `key` ，第二个成员为 `value` ，详见 [std::unordered_map::find](http://www.cplusplus.com/reference/unordered_map/unordered_map/find/)