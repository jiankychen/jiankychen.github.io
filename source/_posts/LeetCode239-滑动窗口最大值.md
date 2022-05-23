---
title: LeetCode 239. 滑动窗口最大值
tags:
  - 队列
categories:
  - LeetCode
cover: false
abbrlink: 3ba13828
date: 2022-05-23 16:49:23
---

[239. Sliding Window Maximum](https://leetcode.cn/problems/sliding-window-maximum/)


You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

 

**Example 1:**

    Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
    Output: [3,3,5,5,6,7]
    Explanation: 
    Window position                Max
    ---------------               -----
    [1  3  -1] -3  5  3  6  7       3
    1 [3  -1  -3] 5  3  6  7       3
    1  3 [-1  -3  5] 3  6  7       5
    1  3  -1 [-3  5  3] 6  7       5
    1  3  -1  -3 [5  3  6] 7       6
    1  3  -1  -3  5 [3  6  7]      7


**Example 2:**

    Input: nums = [1], k = 1
    Output: [1]
 

**Constraints:**

 - $1 \le$ `nums.length` $\le 10^5$
 - $-10^4 \le$ `nums[i]` $\le 10^4$
 - $1 \le$ `k` $\le$ `nums.length`


## 思路：

数组长为 `nums.size()` ，窗口长为 `k` ，一共有 `nums.size() - k + 1` 个窗口

若采用暴力法，对每个窗口求最大值需要 $O(k)$ 的时间复杂度，则总时间复杂度为 $O(n k)$

针对本题的数据，暴力法会超时

本题有三种求解思路：可参考 [力扣官方题解](https://leetcode.cn/problems/sliding-window-maximum/solution/dong-hua-yan-shi-dan-diao-dui-lie-239hua-hc5u/)
 - 优先队列（堆）
 - 单调队列
 - 分块 + 预处理


## Method 1: 优先队列

可以利用 最大化堆，即，根节点元素值最大的二叉堆， 来维护窗口的最大值

> 有关 最大化堆 ，可参考 [优先级队列](https://jiankychen.github.io/posts/a21107fc)

算法流程：

1. 定义一个 优先队列 ，其中，队列每个元素都是一个二元组，存储了 `nums` 数组元素值和元素下标，即：`priority_queue<pair<int, int>> que;`

2. 将 `nums` 数组的前 `k` 个元素（及下标）加入到优先队列中

3. 遍历 `nums` 数组元素下标 `i`（从第 `k` 位元素开始），即，窗口的右边界

     - 将 `(nums[i], i)` 加入到 `que` 中
     - 若 `que` 根节点所对应的数组元素下标（即，`que.top().second`） 小于 窗口的左边界（即，`i - k + 1`），则这个最大值不在窗口范围内，而是在窗口左侧，将其从 `que` 中移除
     - 将 `que` 根节点所对应的数组元素（即，`que.top().first`）加入到目标数组中

代码实现：

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    priority_queue<pair<int, int>> que;
    vector<int> res(nums.size() - k + 1, 0);
    // 将前 k 个元素添加到优先队列
    for (int i = 0; i < k; i++)
        que.emplace(nums[i], i);
    // 将当前窗口最大值添加到目标数组
    res[0] = que.top().first;
    for (int i = k; i < nums.size(); i++) {
        // 将当前元素添加到优先队列
        que.emplace(nums[i], i);
        // 当前队列的最大值不在窗口内，将其移除
        while (que.top().second < i - k + 1)
            que.pop();
        // 将当前窗口最大值添加到目标数组
        res[i - k + 1] = que.top().first;
    }
    return res;
}
```

时间复杂度：$O(n \log{n})$ ，其中，$n$ 是数组 `nums` 的长度
 - 遍历数组的时间复杂度为 $O(n)$
 - 将元素添加到优先队列的时间复杂度为 $O(\log{n})$ （在最坏情况下，数组 `nums` 中的元素单调递增，优先队列中包含了所有元素）

空间复杂度：$O(n)$ ，这里仅考虑了优先队列所需空间，忽略了目标数组所需空间

参考：[力扣官方题解：滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/solution/dong-hua-yan-shi-dan-diao-dui-lie-239hua-hc5u/)


## Method 2: 单调队列

双向队列：队列的两端都可以 入队 / 出队

单调队列：从队首到队尾，元素单调递减，或递增

本题可定义一个 元素单调递减的单调队列 ，以维护窗口内的最大值（即，队列的首端元素为队列最大值）
 - 若队首元素不在窗口内（即，对应数组下标小于窗口左边界），将其移除
 - 若队首元素在窗口内，则该元素即为窗口内元素的最大值

为实现队列元素的单调递减，我们在添加元素时，需做以下考虑：
 - 若队列尾端元素小于等于当前元素，则将队尾元素移除（因为队尾元素对应的数组下标在当前元素的左侧，当队尾元素小于当前元素时，其不可能成为当前窗口及后续滑动窗口的最大值，可将其从队列中永久移除）
 - 直到队列尾端元素大于当前元素时，才将当前元素添加到队列中


算法流程：

1. 定义一个双向队列：`deque<int> que;` ，用以实现单调队列
2. 遍历数组 `nums` 数组元素下标，定义为窗口的右边界 `right`
    - 若队列不为空且当前元素大于等于队尾元素，将队尾元素移除，直到队列为空或当前元素小于新的队尾元素
     - 将当前元素添加到队尾
     - 若队首元素的下标小于滑动窗口左侧边界，将其从队首移除，直到队首元素在窗口中为止
     - 若长为 `k` 的窗口已形成（即，`right >= k - 1`），则将窗口最大值（即，队首元素）添加到目标数组

> 特别地，为便于操作，我们可将数组元素的下标存放在队列中

代码实现：

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> que;     // 单调队列（从队首到队尾，元素单调递减）
    vector<int> res;    // 目标数组
    for (int left = 0, right = 0; right < nums.size(); right++) {
        // 队列不为空且当前考察元素大于等于队尾元素，将队尾元素移除
        while (!que.empty() && nums[que.back()] <= nums[right])
            que.pop_back();
        // 将当前元素的下标添加到队尾
        que.push_back(right);
        // 窗口左边界
        left = right - k + 1;
        // 将小于 left 的元素下标从队列移除
        while (que.front() < left)
            que.pop_front();
        // 队列首端对应元素就是窗口最大值
        if (right >= k -1)
            res.push_back(nums[que.front()]);
    }
    return res;
}
```

时间复杂度：$O(n)$，其中，$n$ 为数组长度

空间复杂度：$O(k)$，其中，$k$ 为窗口长度
 - 队列最多有 $k + 1$ 个元素，故，队列所需空间为 $O(k)$
 - 忽略了目标数组所需空间


参考：
 - [力扣官方题解：滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/solution/dong-hua-yan-shi-dan-diao-dui-lie-239hua-hc5u/)
 - [编程狂想曲：动画演示 单调队列 239.滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/solution/dong-hua-yan-shi-dan-diao-dui-lie-239hua-hc5u/)