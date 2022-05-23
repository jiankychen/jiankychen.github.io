---
title: LeetCode 347. 前 K 个高频元素
tags:
  - 队列
categories:
  - LeetCode
cover: false
abbrlink: f382432a
date: 2022-05-23 20:39:36
---

[347. Top K Frequent Elements](https://leetcode.cn/problems/top-k-frequent-elements/)


Given an integer array `nums` and an integer `k`, return the **`k` most frequent elements**. You may return the answer **in any order**.

 

**Example 1:**

    Input: nums = [1,1,1,2,2,3], k = 2
    Output: [1,2]

**Example 2:**

    Input: nums = [1], k = 1
    Output: [1]
 

**Constraints:**

 - $1 \le$ `nums.length` $\le 10^5$
 - `k` is in the range `[1, the number of unique elements in the array]`.
 - It is **guaranteed** that the answer is **unique**.
 

**Follow up:** Your algorithm's time complexity must be better than `O(n log n)`, where `n` is the array's size.


## 思路

解题基本思路：

 - 统计元素出现频率：利用 **哈希 map** 来统计数组中各元素出现的次数
 - 对频率排序：利用 **优先级队列** 来对频率进行排序
 - 根据排序结果找出前 K 个高频元素

> 题目只需找出前 K 个高频元素，因此可采用固定大小为 K 的 优先级队列 维护一个长为 K 的有序序列即可，而无需采用 vector 容器对所有元素的频次进行排序



## Method: 哈希 map + 优先级队列

由于优先级队列（堆）只能移除队首元素，本题采用 **最小化堆（小顶堆）** ，以便移除频次最少的元素

算法流程：

1. 定义一个哈希表（`unordered_map`），用于统计数组元素的出现频次，其中，key 为元素值，value 为频次

2. 定义一个优先级队列，遍历哈希表：
    - 将哈希表的键值对添加到队列
    - 若队列长度大于 K ，则从队列中移除频次最少的元素

3. 从优先级队列中提取出前 K 个高频元素的元素值，将其填充到目标数组中



代码实现：

```cpp
// 最小化堆的比较函数
class comp {
public:
    bool operator() (const pair<int, int> &LHS, const pair<int, int> &RHS) {
        return LHS.second > RHS.second;
    }
};

class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        // 统计数字出现的频次
        unordered_map<int, int> hash;
        for (int i = 0; i < nums.size(); i++)
            hash[nums[i]]++;
        // 利用固定大小为 K 的优先队列扫描哈希表
        priority_queue<pair<int, int>, vector<pair<int, int>>, comp> que;
        for (auto it = hash.begin(); it != hash.end(); it++) {
            que.push(*it);
            if (que.size() > k)
                que.pop();
        }
        // 提取出前 K 个高频元素（升序填充到目标数组）
        vector<int> res(k, 0);
        for (int i = 0; i < k; i++) {
            res[i] = que.top().first;
            que.pop();
        }
        return res;
    }
};
```

时间复杂度：$O(n \log{k})$ ，其中，$n$ 为数组长度，$k$ 为输出高频元素数
 - 遍历哈希表的最坏时间复杂度为 $O(n)$
 - 优先级队列的入队/出队时间复杂度为 $O(\log{k})$

空间复杂度：$O(n)$

参考：
 - [cplusplus：std::priority_queue](http://www.cplusplus.com/reference/queue/priority_queue/)
 - [代码随想录：前 K 个高频元素](https://www.programmercarl.com/0347.%E5%89%8DK%E4%B8%AA%E9%AB%98%E9%A2%91%E5%85%83%E7%B4%A0.html)