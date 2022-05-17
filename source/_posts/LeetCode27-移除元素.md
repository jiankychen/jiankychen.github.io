---
title: LeetCode 27. 移除元素
tags:
  - 数组
  - 双指针
categories:
  - LeetCode
cover: false
abbrlink: c425967e
date: 2022-03-29 23:01:45
---

[LeetCode 27. Remove Element](https://leetcode.cn/problems/remove-element/)

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` *in-place*. The relative order of the elements may be changed.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` *after placing the final result* in the first `k` slots of nums.

Do not allocate extra space for another array. You must do this by **modifying the input array** *in-place* with `O(1)` extra memory.


**Custom Judge**:

The judge will test your solution with the following code:
```cpp
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```
If all assertions pass, then your solution will be accepted.

**Example 1:**

    Input: nums = [3,2,2,3], val = 3
    Output: 2, nums = [2,2,_,_]

    Explanation: Your function should return k = 2, with the first two elements of nums being 2.
    It does not matter what you leave beyond the returned k (hence they are underscores).


**Example 2:**

    Input: nums = [0,1,2,2,3,0,4,2], val = 2
    Output: 5, nums = [0,1,4,0,3,_,_,_]

    Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
    Note that the five elements can be returned in any order.
    It does not matter what you leave beyond the returned k (hence they are underscores).
 

**Constraints**:
 - $0 \le$ `nums.length` $\le 100$
 - $0 \le$ `nums[i]` $\le 50$
 - $0 \le$ `val` $\le 100$


## Method 1: 双指针

`slow` 对应 将要被覆盖的位置，`fast` 对应 当前搜索位置，均初始化为 `0`

`fast` 右移，并进行如下操作，直到 `fast == nums.size() - 1` ：

 - 如果 `nums[fast] == val` ，跳过该元素

 - 如果 `nums[fast] != val` ，令 `nums[slow] = nums[fast]` ，并让 `l` 右移一位

代码实现：

```cpp
int removeElement(vector<int>& nums, int val) {
    int n = nums.size();
    int slow = 0;
    for (int fast = 0; fast < n; fast++)
        if (nums[fast] != val) // 对值不为 val 的元素进行移位，值为 val 则跳过
            nums[slow++] = nums[fast];
    return slow;               // slow 即为 新数组的长度
}
```

时间复杂度 ：$O(n)$，其中 $n$ 为数组的长度

空间复杂度 ：$O(1)$


## Method 2: 双指针优化
如果要移除的元素恰好在数组的开头，方法一需要把每一个元素都左移一位

可以定义两个指针，初始时分别指向数组的首尾，向中间移动遍历该序列（题目允许改变元素的顺序）


算法流程：

执行以下循环，直到 `left` 与 `right` 重合

 - 判断 `left` 的元素是否等于 `val` 
   - 若是，将 `right` 指向的元素复制到 `left` 的位置，然后 `right` 左移一位
   - 若否，`left` 右移一位
  
> 注意这里的 `right` 指向的元素也有可能是 `val` ，此时：
>  - 可以选择将 `val` 赋值给 `left` ，然后 `right` 左移（在这种情况下，赋值后 `left` 位置的元素仍为 `val` ，`left` 不会移动）
>  - 也可以选择跳过该元素，即， `right` 直接左移

代码实现：

```cpp
int removeElement(vector<int>& nums, int val) {
    int left = 0, right = nums.size() - 1;
    while (left < right) {
        if (nums[left] == val)
            nums[left] = nums[right--];
        else
            left++;
    }
    return left;
}
```

时间复杂度 ：$O(n)$

空间复杂度 ：$O(1)$

与 Method 1 相比，Method 2 避免了值不为 `val` 元素的重复赋值操作