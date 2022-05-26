---
title: LeetCode 145. 二叉树的后序遍历
tags:
  - 二叉树
categories:
  - LeetCode
math: true
abbrlink: 4e96229e
date: 2022-05-26 15:55:15
---

[LeetCode 145. Binary Tree Postorder Traversal](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)


Given the `root` of a binary tree, return *the postorder traversal of its nodes' values.*


**Example 1:**

![](LeetCode145-二叉树的后序遍历/1.jpg)

    Input：root = [1,null,2,3]
    Output：[3,2,1]


**Example 2:**

    Input: root = []
    Output: []


**Example 3:**

    Input: root = [1]
    Output: [1]


**Constraints:**

 - The number of nodes in the tree is in the range `[0, 100]`.
 - $-100 \le$ `Node.val` $\le 100$
 

**Follow up:** Recursive solution is trivial, could you do it iteratively?

## Method 1: 递归

代码实现：

```cpp
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    postorder(root,res);         // 中序遍历
    return res;
}

void postorder(TreeNode* root, vector<int> &res) {
    if (root == nullptr) return;
    postorder(root->left,res);   // 左子树
    postorder(root->right,res);  // 右子树
    res.push_back(root->val);    // 根节点
}
```

## Method 2: 迭代


[^_^]: 这部分被注释掉了
    ### Method 3: Morris遍历

[力扣官方题解：二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/solution/er-cha-shu-de-hou-xu-bian-li-by-leetcode-solution/)