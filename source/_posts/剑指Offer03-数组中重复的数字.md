---
title: 剑指 Offer 03. 数组中重复的数字
date: 2022-04-15 20:00:24
tags:
 - 排序
categories:
cover: false
---

找出重复的数字

输入描述：
 - 第一行是整数 `n` ，表示自然数的个数
 - 第 `2` 行是输入的数组

输出描述：
- 每行输出两个整数，分别是自然数和该数出现的次数，用一个空格隔开


样例：

    输入：
    8
    [2, 4, 2, 4, 5, 100, 2, 100]

    输出：
    2 3
    4 2
    5 1
    100 2



## Method : 快速排序

将输入数据进行排序，使得相同数字处在连续的位置上

然后统计各数字的出现次数

```cpp
void quick_sort (int &a[], int x, int y) {

    int pivot = y;
    int pos = x;
    for (int i = x; i < y; i++)
        if (a[i] < pivot) {
            swap(a[i], a[pos]);
            pos++;
        }
    swap(a[y], a[pos]);
    
    if (x < pos - 1)
        quick_sort(a, x, pos - 1);
    if (pos + 1 < y)
        quick_sort(a, pos + 1, y);
}

void findRepeatNumber(int n, int &a[]) {

    // 快速排序，函数传递的参数是指针，分别指向数组的首地址和尾地址
    quick_sort(a, 0, n - 1);

    // 统计数字
    int num = 1;    // 计数器，统计出现次数
    for (int i = 2; i <= n; i++)
        if (a[i - 1] == a[i])   // 当前数字与前一个数字相同，更新计数器
            num++;
        else {  // 当前数字与前一个不同，输出前一个数字的值及其出现次数，并重置计数器
            cout << a[i - 1] << " " << num << endl;
            num = 1;
        }
    cout << a[n] << " " << num << endl; // 输出最后一个数的值及其出现次数
}
```