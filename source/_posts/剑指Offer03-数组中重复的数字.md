---
title: 剑指Offer 03. 数组中重复的数字
tags:
  - 数组
  - 排序
  - 哈希表
categories:
  - LeetCode
cover: false
abbrlink: b5e5ca5b
date: 2022-04-19 16:32:45
---

[剑指Offer 03](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

在一个长度为 `n` 的数组 `nums` 里，所有数字都在 0 ~ n-1 的范围内，请找出数组中任意一个重复的数字

**Example 1：**

    输入：[2, 3, 1, 0, 2, 5, 3]
    输出：2 或 3 
 

**Constraints：**
 - $2 \le$ `n` $\le 100000$

## stable_sort 函数

```cpp
int findRepeatNumber(vector<int>& nums) {
    // stable_sort 函数（归并排序的优化实现）
    int n = nums.size();
    stable_sort(nums.begin(), nums.end());

    // 检查是否有重复
    for (int i = 1; i < n; i++) {
        if (nums[i] == nums[i - 1])
            return nums[i];
    }
    return -1;
}
```

注：这里若采用快速排序或归并排序，将会超出时间限制


## 哈希表
使用集合存储已经遇到的数字，如果遇到的一个数字已经在集合中，则当前的数字是重复数字

算法流程：

1. 初始化 `ordered_map` 容器，记为 `map`
2. 遍历数组 `nums` 中的每个数字 `num` ：
     - 当 `num` 在 `map` 中，即，`map[num] >= 2`，直接返回 `num`
     - `num` 不在 `map` 中，`map[num]` 加 `1`

3. 若遍历结束时未发现重复数字，返回 `-1`

```cpp
int findRepeatNumber(vector<int>& nums) {
    unordered_map<int, bool> map;
    for (auto num : nums) {
        if(map[num]) // 若 map[num] = 1，则数字 num 重复
            return num;
        map[num]++;
    }
    return -1;
}
```

时间复杂度：
 - $O(n)$，遍历数组

空间复杂度：
 - $O(n)$，哈希表占用额外空间



## 原地交换

注意题目说明：`在一个长度为 n 的数组 nums 里的所有数字都在 0 ~ n-1 的范围内`，因此，可以通过交换操作，建立数组元素 索引 与 值 的映射关系

基本思想：遍历数组 `nums` ，第一次遇到数字 `x` 时，将其交换至索引 `x` 处；而当第二次遇到数字 `x` 时，一定有 `nums[x] = x` ，得到重复数字


算法流程：

1. 遍历数组 `nums` ，设索引初始值为 `i = 0` ：
     - 若 `nums[i] = i` ：说明此数字已在对应索引位置，无需交换，因此跳过

     - 若 `nums[nums[i]] = nums[i]` ：代表索引 `nums[i]` 处和索引 `i` 处的元素值都为 `nums[i]` ，即找到一组重复值，直接返回值 `nums[i]`

     - 否则：交换索引为 `i` 和索引为 `nums[i]` 的两个元素，将此数字交换至对应索引位置

2. 若遍历完毕尚未返回，则返回 `-1`

代码实现：
```cpp
int findRepeatNumber(vector<int>& nums) {
    int i = 0, n = nums.size();
    while (i < n) {
        if (nums[i] == i) { // 注意：仅这里进行 i++ ，后续两种情况不进行 i++ 
            i++;
            continue;
        }
        else if (nums[nums[i]] == nums[i])  // 直接返回，无需再操作 i
            return nums[i];
        else                                // 须对 nums[nums[i]] 作进一步的交换操作，将其放至值所对应的索引处，故而此处不进行 i++
            swap(nums[i], nums[nums[i]]);
    }
    return -1;
}
```

时间复杂度：
 - $O(N)$，遍历数组

空间复杂度：
 - $O(1)$，使用常数复杂度的额外空间

参考：[Krahets](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/mian-shi-ti-03-shu-zu-zhong-zhong-fu-de-shu-zi-yua/)