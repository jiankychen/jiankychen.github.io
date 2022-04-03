---
title: LeetCode 283. 移动零
date: 2022-03-14 20:53:59
tags:
 - 双指针
 - 数组
categories:
 - LeetCode
cover: false
---

[LeetCode 283](https://leetcode-cn.com/problems/move-zeroes/)

Given an integer array nums, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this **in-place** without making a copy of the array.

Example 1:

    Input: nums = [0,1,0,3,12]
    Output: [1,3,12,0,0]


Example 2:

    Input: nums = [0]
    Output: [0]



## Method 1: 翻转数组
1. 设索引为`i`，若`nums[i]`为`0`，则翻转第`i`至第`nums.size()-1-num`位。
2. 翻转第`i`位至第`nums.size()-2-num`位，将非零元素翻转回原来的顺序。注意，经过这次翻转，需要重新对第`i`位进行判断，因为新数组的第`i`位实际上是最初数组的第`i+1`位

> 因为末尾已有`num`个`0`，需要将第`i`位上的`0`放到第`n-1-num`位上，才能使得两次翻转后非零元素的顺序不变 并且 之前找到的`0`仍在末尾。

```cpp
// 翻转left至right之间的所有元素
void reverse(vector<int>& nums, int left, int right){
    while (left < right)
    {
        swap(nums[left], nums[right]);
        left++;
        right--;
    };
}
```

```cpp
void moveZeroes(vector<int>& nums) {
    int n = nums.size(), i = 0, num = 0;
    while (i < n - num)
    {
        if (nums[i] == 0)
        {
            reverse(nums, i, n - 1 - num);      // 末尾已有num个0，将第i位上的0放到第n-1-num位上，执行翻转操作
            reverse(nums, i, n - 2 - num);      // 保持末尾的num+1个0不动，翻转第i位至第n-2-num位，可使非零元素恢复原顺序
            num++;                  // 因为移动了一个0到最末尾，故而i可以少移动一位
        }
        else
            i++;
    };
}
```

## Method 2: 双指针
使用双指针，左指针指向当前的搜索位置，右指针指向补0位置。当左指针搜索到0时，将左指针与右指针之间的数左移一位，右指针位置赋0，同时右指针左移一位。
```cpp
void moveZeroes(vector<int>& nums) {
    int n = nums.size();
    int i = 0, j = n - 1;
    while (i < j)
    {
        if (nums[i] == 0)    // 第i位为0时，将第i+1至j位均向左移动一位，第j位赋值0，向左移动指针j
        {
            for (int k = i + 1; k <= j; k++)
                nums[k-1] = nums[k];
            nums[j] = 0;
            j--;
        }
        else i++;           // 第i位不为0时，指针i继续向右移动
    };
}
```

## Method 3: 双指针（两次遍历）
[题解](https://leetcode-cn.com/problems/move-zeroes/solution/dong-hua-yan-shi-283yi-dong-ling-by-wang_ni_ma/)

指针`i`和`j`，指针`i`用于遍历搜索，指针`j`用来记录当前有多少非`0`元素。

第一次遍历的时候每遇到一个非`0`元素就将其往数组左边挪，第一次遍历完后，`j`指针的下标就指向了最后一个非`0`元素下标。

第二次遍历的时候，起始位置就从`j`开始到结束，将剩下的这段区域内的元素全部置为`0`。
```cpp
void moveZeroes(vector<int>& nums) {
    // 指针j记录非0元素数，只要nums[i]非零就都赋给nums[j]，然后i,j向右移位，循环终止时，j所在位置是最后一个非0元素的下一位，即，赋0的起点
    int j = 0;
    for (int i = 0; i < nums.size(); i++)
    {
        if (nums[i] != 0)
        {
            nums[j] = nums[i];
            j++;
        }
    };
    // 从第j位开始赋0
    for (int i = j; i < nums.size(); i++)
        nums[i] = 0;
};
```

## Method 4: 双指针（一次遍历）
[题解](https://leetcode-cn.com/problems/move-zeroes/solution/dong-hua-yan-shi-283yi-dong-ling-by-wang_ni_ma/)

这里参考了快速排序的思想，快速排序首先要确定一个待分割的元素做中间点`x`，然后把所有小于等于`x`的元素放到`x`的左边，大于`x`的元素放到其右边。

这里用`0`当做这个中间点，把不等于`0`的放到中间点的左边，等于`0`的放到其右边。

指针`i`用于遍历搜索，指针`j`用来记录当前有多少非`0`元素，只要`nums[i]!=0`，我们就交换`nums[i]`和`nums[j]`（也可以直接将nums[i]赋给），并且将`i`,`j`右移。

Code 1：
```cpp
void moveZeroes(vector<int>& nums) {
    int j = 0;
    for (int i = 0; i < nums.size(); i++)
    {
        if (nums[i] != 0)
        {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
            j++;
        }
    };
}
```

Code 2：
```cpp
void moveZeroes(vector<int>& nums) {
    int j = 0;
    for (int i = 0; i < nums.size(); i++)
    {
        if (nums[i] != 0)
        {
            if (i > j)  // #1
            {
                nums[j] = nums[i];
                nums[i] = 0;
            }
            j++;
        }
    };
}
```

与`Code 1`相比，`Code 2`优化`#1`处。`Code 2`避免了数组开头的非零元素的交换，也就是阻止`i == j`时交换。当`i > j` 时，只需要把 `i` 的值赋值给`j`并把原位置的值置`0`。同时这里也把交换操作换成了赋值操作，理论上能更节省时间。