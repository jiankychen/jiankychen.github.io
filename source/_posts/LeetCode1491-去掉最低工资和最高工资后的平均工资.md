---
title: LeetCode 1491. 去掉最低工资和最高工资后的平均工资
date: 2022-03-15 22:26:56
tags:
 - 数组
categories:
 - LeetCode
cover: false
---

[LeetCode 1491](https://leetcode-cn.com/problems/average-salary-excluding-the-minimum-and-maximum-salary/)

You are given an array of **unique** integers salary where `salary[i]` is the salary of the `i`-th employee.

Return the average salary of employees excluding the minimum and maximum salary. Answers within `10^-5` of the actual answer will be accepted.

 

**Example 1:**

    Input: salary = [4000,3000,1000,2000]
    Output: 2500.00000
    Explanation: Minimum salary and maximum salary are 1000 and 4000 respectively.
    Average salary excluding minimum and maximum salary is (2000+3000) / 2 = 2500


**Example 2:**

    Input: salary = [1000,2000,3000]
    Output: 2000.00000
    Explanation: Minimum salary and maximum salary are 1000 and 3000 respectively.
    Average salary excluding minimum and maximum salary is (2000) / 1 = 2000



## Method：前缀和

逐个比较，更新最大值和最小值，并累加求和，最后再减去最大值和最小值。注意，求平均值时，被除数或者除数要转换成 `double` 型，这样才能保留小数点后的计算结果。

```cpp
double average(vector<int>& salary) {
    int min = salary[0], max = salary[0], sum = 0;
    for (int i = 0; i < salary.size(); i++)
    {
        if (salary[i] > max) max = salary[i];
        else if (salary[i] < min) min = salary[i];
        sum += salary[i];
    };
    double ans = double(sum - max - min) / (salary.size() - 2); // 注意这里被除数或除数要变成double型
    return ans;
```


时间复杂度：$O(n)$ ，选取最大值、最小值和求和的过程的时间代价都是 $O(n)$ ，故渐进时间复杂度为 $O(n)$

空间复杂度：$O(1)$ ，这里只用到了常量级别的辅助空间。