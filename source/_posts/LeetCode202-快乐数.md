---
title: LeetCode 202. 快乐数
date: 2022-05-03 19:46:27
tags:
 - 哈希表
 - 双指针
categories:
 - LeetCode
cover: false
---

[LeetCode 202. Happy Number](https://leetcode-cn.com/problems/happy-number/)

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

 - Starting with any positive integer, replace the number by the sum of the squares of its digits.
 - Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
 - Those numbers for which this process **ends in 1** are happy.


Return `true` if `n` is a happy number, and `false` if not.

 

**Example 1:**

    Input: n = 19
    Output: true
    Explanation:
    12 + 92 = 82
    82 + 22 = 68
    62 + 82 = 100
    12 + 02 + 02 = 1


**Example 2:**

    Input: n = 2
    Output: false
 

**Constraints:**

 - $1 \le$ `n` $\le 2^{31} - 1$


## 思路

注意到题目说明：
 - 若各位平方和最终等于 1 ，则 `n` 为快乐数
 - 若各位平方和的值循环出现，则 `n` 不是快乐数

因此，本题的解题关键在于，在求和过程中，判断数字是否重复出现
 - 若重复出现，则该数字必定不是快乐数
 - 若求和结果为 1，则该数字是快乐数

## Method 1: 哈希 set

[std::unordered_set](http://www.cplusplus.com/reference/unordered_set/unordered_set/?kw=unordered_set)

考虑用哈希表 `unordered_set` 记录求和过程中出现的数字，并判断其是否重复出现

另，需注意 `n` 的各位数字平方和的计算
 - 计算个位数字的平方
 - 取个位以外的部分
 - 重复以上两步


代码实现：

```cpp
int getSum(int n) { // 计算 n 中各位的平方和
    int sum = 0;
    while (n) {
        sum += (n % 10) * (n % 10); // 取 n 的个位数，求其平方，再累加到 sum 上
        n /= 10;                    // 取个位数以外的部分
    }
    return sum;
}

bool isHappy(int n) {
    unordered_set<int> record;
    while (1) {
        int sum = getSum(n);
        if (sum == 1)
            return 1;
        if (record.find(sum) != record.end()) // sum 重复出现
            return 0;
        else
            record.insert(sum);               // sum 第一次出现
        n = sum;                              // 更新当前计算数字
    }
}
```

[代码随想录：快乐数](https://www.programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html)

时间复杂度：$O(\log{n})$
 - 查找给定数字的下一个值的时间复杂度为 $O(\log{n})$ ，因为数字的位数由 $O(\log_{10}{n}) = O(\log{n})$ 确定
 - 总体的时间复杂度还需考虑循环过程中的数字个数，这里简单起见仅考虑计算下一个值的时间复杂度

空间复杂度：$O(\log{n})$



## Method 2: 快慢指针

可以采用类似于 [LeetCode 142. 环形链表 II](https://jiankychen.github.io/2022/04/29/leetcode142-huan-xing-lian-biao-ii/) 的方法，定义快慢指针，判断是否存在环
 - 若存在环，则快慢指针一定会在环内相遇，即，数 `n` 不是快乐数
 - 否则，快指针会比慢指针先到达数字 1 ，即，数 `n` 是快乐数

[力扣官方题解：快乐数](https://leetcode-cn.com/problems/happy-number/solution/kuai-le-shu-by-leetcode-solution/)