---
title: LeetCode 744. 寻找比目标字母大的最小字母
tags:
  - 数组
  - 二分查找
categories:
  - LeetCode
cover: false
abbrlink: 7e8f6f39
date: 2022-04-19 15:53:36
---

Given a characters array `letters` that is sorted in **non-decreasing** order and a character `target`, return the smallest character in the array that is larger than `target`.

Note that the letters wrap around.
 - For example, if `target == 'z'` and `letters == ['a', 'b']`, the answer is `'a'`.

**Example 1:**

    Input: letters = ["c","f","j"], target = "a"
    Output: "c"


**Example 2:**

    Input: letters = ["c","f","j"], target = "c"
    Output: "f"


**Example 3:**

    Input: letters = ["c","f","j"], target = "d"
    Output: "f"


**Example 4:**

    Input: letters = ["c","f","j"], target = "z"
    Output: "c"


**Constraints:**
 - $2 \le$ `letters.length` $\le 10^4$
 - `letters[i]` is a **lowercase** English letter.
 - `letters` is sorted in non-decreasing order.
 - `letters` contains at least two different characters.
 - `target` is a lowercase English letter.


## Method: 二分查找

旨在找出大于 `target` 的最小字母，可以考虑二分查找算法

二分查找的基本思路：
 1. 考虑 `while` 循环终止条件为 `left = right`
    - 如果 `letters[mid] > target` ，则所求字母在 `[left, mid]` 区间，所以更新 `right = mid`
    - 如果 `letters[mid] <= target`，则所求字母在 `[mid + 1, right]` 区间，所以更新 `left = mid + 1`
 2. 循环结束时，需要判断 `letters[left]` 是否大于 `target`
     - 若是，输出 `letters[left]`
     - 若否，则所有元素均不大于 `target` ，输出 `letters[0]`

```cpp
// 找出大于目标值的最小元素索引
int LowerBoundSearch(vector<char> letters, char target) {
    int left = 0, right = letters.size() - 1;
    while (left < right) {
        int mid = left + ((right - left) >> 1); // 注意：+ 号优先级高于位运算 >> ，需要将 (right - left) >> 1 括起来
        if (letters[mid] > target)
            right = mid;
        else
            left = mid + 1;
    }
    // 判断 letters[left] 是否大于 target
    if (letters[left] > target)
        return left;
    else  // 所有字母都比 target 小（注意：字母在 letters 中依序循环出现，返回 0 ）
        return 0;
    // // 等价于
    // return letters[left] > target ? left : 0;
}

char nextGreatestLetter(vector<char>& letters, char target) {
    int index = LowerBoundSearch(letters, target);
    return letters[index];
}
```