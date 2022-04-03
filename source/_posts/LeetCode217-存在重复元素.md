---
title: LeetCode 217. 存在重复元素
date: 2022-03-26 21:38:39
tags:
 - 哈希表
 - 数组
categories:
 - LeetCode
cover: false
---

[LeetCode 217](https://leetcode-cn.com/problems/contains-duplicate/)

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is **distinct**.

 

Example 1:

    Input: nums = [1,2,3,1]
    Output: true


Example 2:

    Input: nums = [1,2,3,4]
    Output: false


Example 3:

    Input: nums = [1,1,1,3,3,4,3,2,4,2]
    Output: true
 

Constraints:

$1 \le nums.length \le 10^5$
$-10^9 \le nums[i] \le 10^9$


## Method 1: 排序

若采取两个循环嵌套进行暴力查找，时间复杂度为 $O(N^2)$，将超出时间限制

可以先对数组 `nums` 进行排序，检查相邻两元素是否相等

```cpp
bool containsDuplicate(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    int n = nums.size();
    for (int i = 0; i < n - 1; i++) {
        if (nums[i] == nums[i + 1]) return true;
    }
    return false;
}
```

时间复杂度：$O(N \log N)$ ，其中 $N$ 为数组长度。因为需要对数组排序

空间复杂度：$O(\log N)$ ，其中 $N$ 为数组长度。注意我们在这里应当考虑递归调用栈的深度


## Method 2: 哈希表 set
可以尝试将数组中每个元素都插入到哈希表 `unordered_set` 中：如果插入一个元素时发现该元素已经存在于哈希表中，则说明存在重复的元素

```cpp
bool containsDuplicate(vector<int>& nums) {
    unordered_set<int> set;
    for (auto c : set) { // c 是 int 型
        if (set.find(c) != set.end()) return true;  // 找到 c 的位置不在尾后，即，哈希表中存在元素 c
        set.insert(c);   // 将 c 插入到哈希表中
    }
    return false;
```

时间复杂度：$O(N)$，其中 $N$ 为数组的长度

空间复杂度：$O(N)$，其中 $N$ 为数组的长度


## Method 3: 哈希表 map
也可以直接用 `unordered_map` 容器统计每一个元素出现的次数，然后遍历整个 `unordered_map` 容器查看是否有元素出现的次数大于等于2

```cpp
bool containsDuplicate(vector<int>& nums) {
    unordered_map<int,int> map;
    for(int num : nums) {
        map[num]++;
        if(map[num] >= 2) return true;
    }
    return false;
}
```