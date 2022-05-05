---
title: LeetCode 167. 两数之和 II - 输入有序数组
date: 2022-03-14 23:06:59
tags:
 - 双指针
 - 二分查找
 - 数组
categories:
 - LeetCode
cover: false
---

[LeetCode 167](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

Given a **1-indexed** array of integers numbers that is already sorted ***in non-decreasing order***, find two numbers such that they add up to a specific target number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return the indices of the two numbers, `index1` and `index2`, **added by one** as an integer array `[index1, index2]` of *length 2*.

The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.

Your solution must use only constant extra space.

 

**Example 1:**

    Input: numbers = [2,7,11,15], target = 9
    Output: [1,2]
    Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].


**Example 2:**

    Input: numbers = [2,3,4], target = 6
    Output: [1,3]
    Explanation: The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].


**Example 3:**

    Input: numbers = [-1,0], target = -1
    Output: [1,2]
    Explanation: The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].


## Method 1: 双指针
初始时两个指针分别指向第一个元素位置和最后一个元素的位置。每次计算两个指针指向的两个元素之和，并和目标值比较：
1. 如果两个元素之和等于目标值，则发现了唯一解。
2. 如果两个元素之和小于目标值，则将左侧指针右移一位。
3. 如果两个元素之和大于目标值，则将右侧指针左移一位。

移动指针之后，重复上述操作，直到找到答案。
```cpp
vector<int> twoSum(vector<int>& numbers, int target) {
    int i = 0, j = numbers.size() - 1;
    while (i < j)
    {
        if (numbers[i] == target - numbers[j])
            return {i + 1, j + 1};
        else if (numbers[i] < target - numbers[j])
            i++;
        else
            j--;
    };
    return {-1, -1};
}
```

时间复杂度：$O(n)$ ，其中 $n$ 是数组的长度。两个指针移动的总次数最多为 $n$ 次

空间复杂度：$O(1)$


## Method 2: 二分查找
在数组中找到两个数，使得它们的和等于目标值，可以首先固定第一个数，然后寻找第二个数，第二个数等于目标值减去第一个数的差。利用数组的有序性质，可以通过二分查找的方法寻找第二个数。为了避免重复寻找，在寻找第二个数时，只在第一个数的右侧寻找。
```cpp
vector<int> twoSum(vector<int>& numbers, int target) {
    for (int i = 0; i < numbers.size(); ++i) {
        int low = i + 1, high = numbers.size() - 1;
        while (low <= high) {
            int mid = (high - low) / 2 + low;
            if (numbers[mid] == target - numbers[i]) {
                return {i + 1, mid + 1};
            } else if (numbers[mid] > target - numbers[i]) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
    }
    return {-1, -1};
}
```


时间复杂度：$O(n \log{n})$，其中 $n$ 是数组的长度
 - 需要遍历数组一次确定第一个数，时间复杂度是 $O(n)$
 - 寻找第二个数使用二分查找，时间复杂度是 $O(\log{n})$

空间复杂度：$O(1)$


[题解：推荐使用双指针方法](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/solution/liang-shu-zhi-he-ii-shu-ru-you-xu-shu-zu-by-leet-2/)