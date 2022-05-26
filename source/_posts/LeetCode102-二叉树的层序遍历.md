---
title: LeetCode 102. 二叉树的层序遍历
tags:
  - 二叉树
categories:
  - LeetCode
math: true
abbrlink: c7ba20f5
date: 2022-05-26 15:58:39
---

[LeetCode 102. Binary Tree Level Order Traversal](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

Given the `root` of a binary tree, return *the level order traversal of its nodes' values.* (i.e., from left to right, level by level).

 

**Example 1:**

![](LeetCode102-二叉树的层序遍历/1.jpg)

    Input: root = [3,9,20,null,null,15,7]
    Output: [[3],[9,20],[15,7]]


**Example 2:**

    Input: root = [1]
    Output: [[1]]


**Example 3:**

    Input: root = []
    Output: []
 

**Constraints:**

 - The number of nodes in the tree is in the range `[0, 2000]`.
 - $-1000 \le$ `Node.val` $\le 1000$


### Method: 广度优先搜索

算法思路：

1. 首先将根节点入队

2. 当队列不为空的时候
    - 求当前队列的长度 $s_i$
    - 依次从队列中取 $s_i$ 个元素进行拓展，然后进入下一次迭代

普通广度优先搜索每次只取一个元素拓展，而这里每次取 $s_i$ 个元素，即，第 $i$ 次迭代就得到了二叉树的第 $i$ 层的 $s_i$ 个元素

代码实现：

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector <vector <int>> ret;
    if (!root) {
        return ret;
    }

    queue <TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        int currentLevelSize = q.size();
        ret.push_back(vector <int> ());
        for (int i = 1; i <= currentLevelSize; ++i) {
            auto node = q.front(); q.pop();
            ret.back().push_back(node->val);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
    }
    
    return ret;
}
```

时间复杂度：每个点进队出队各一次，故渐进时间复杂度为 $O(n)$，$n$ 为树的所有节点的个数

空间复杂度：队列中元素的个数不超过 $n$ 个，故渐进空间复杂度为 $O(n)$


[力扣官方题解：二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/solution/er-cha-shu-de-ceng-xu-bian-li-by-leetcode-solution/)