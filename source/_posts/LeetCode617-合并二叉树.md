---
title: LeetCode 617. 合并二叉树
tags:
  - 二叉树
  - 递归
categories:
  - LeetCode
cover: false
abbrlink: 113bbac
date: 2022-03-24 21:51:31
---

[LeetCode 617](https://leetcode-cn.com/problems/merge-two-binary-trees/)

You are given two binary trees `root1` and `root2`.

Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.

**Return the merged tree.**

Note: The merging process must **start from the root nodes** of both trees.

**Example 1:**

![](LeetCode617-合并二叉树/1.jpg)

    Input: root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
    Output: [3,4,5,5,4,null,7]


**Example 2:**

    Input: root1 = [1], root2 = [1,2]
    Output: [2,2]


## Method 1: 深度优先搜索（递归）
思路：

1. 合并节点：计算 `root1->val` 和 `root2->val` 的和

2. 递归，合并节点的左子树：调用函数本身，传递参数 `root1->left` 和 `root2->left` 

3. 递归，合并节点的右子树：调用函数本身，传递参数 `root1->right` 和 `root2->right` 

直到遇到空节点，返回合并结果


```cpp
TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {    // 深度优先搜素（递归）
    if (root1 == nullptr) return root2;
    if (root2 == nullptr) return root1;
    auto merged = new TreeNode(root1->val + root2->val);    // 合并节点
    merged->left = mergeTrees(root1->left,root2->left);     // 递归，合并左子树
    merged->right = mergeTrees(root1->right,root2->right);  // 递归，合并右子树
    return merged;
}
```

时间复杂度：$O(\min(m,n))$，其中 `m` 和 `n` 分别是两个二叉树的节点个数。对两个二叉树同时进行深度优先搜索，只有当两个二叉树中的对应节点都不为空时才会对该节点进行显性合并操作，因此被访问到的节点数不会超过较小的二叉树的节点数

空间复杂度：$O(\min(m,n))$，其中 `m` 和 `n` 分别是两个二叉树的节点个数。空间复杂度取决于递归调用的层数，递归调用的层数不会超过较小的二叉树的最大高度，最坏情况下，二叉树的高度等于节点数

[^_^]: 这里被注释掉了
    LeetCode 用户 "学医写不了脚本" 的留言：官方的`dfs`可能存在问题，直接 `return t1`，导致的是有一部分内存会和 `root1` 共享，修改官方代码后的结果：

    ```cpp
    class Solution {
    private:
        TreeNode* copyTree(TreeNode *node) {
            if (!node) return nullptr;

            auto copy = new TreeNode(node->val);
            copy->left = copyTree(node->left);
            copy->right = copyTree(node->right);

            return copy;
        }

    public:
        TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
            if (t1 == nullptr) {
                return copyTree(t2);
            }
            if (t2 == nullptr) {
                return copyTree(t1);
            }
            auto merged = new TreeNode(t1->val + t2->val);
            merged->left = mergeTrees(t1->left, t2->left);
            merged->right = mergeTrees(t1->right, t2->right);
            return merged;
        }
        
    };
    ```

[题解：递归实现与迭代实现](https://leetcode-cn.com/problems/merge-two-binary-trees/solution/dong-hua-yan-shi-di-gui-die-dai-617he-bing-er-cha-/)


[^_^]: 这里被注释掉了
    ## Method 2: 广度优先搜索

    首先判断两个二叉树是否为空，如果两个二叉树都为空，则合并后的二叉树也为空，如果只有一个二叉树为空，则合并后的二叉树为另一个非空的二叉树。

    如果两个二叉树都不为空，则首先计算合并后的根节点的值，然后从合并后的二叉树与两个原始二叉树的根节点开始广度优先搜索，从根节点开始同时遍历每个二叉树，并将对应的节点进行合并。

    使用三个队列分别存储合并后的二叉树的节点以及两个原始二叉树的节点。初始时将每个二叉树的根节点分别加入相应的队列。每次从每个队列中取出一个节点，判断两个原始二叉树的节点的左右子节点是否为空。如果两个原始二叉树的当前节点中至少有一个节点的左子节点不为空，则合并后的二叉树的对应节点的左子节点也不为空。对于右子节点同理。

    如果合并后的二叉树的左子节点不为空，则需要根据两个原始二叉树的左子节点计算合并后的二叉树的左子节点以及整个左子树。考虑以下两种情况：

     - 如果两个原始二叉树的左子节点都不为空，则合并后的二叉树的左子节点的值为两个原始二叉树的左子节点的值之和，在创建合并后的二叉树的左子节点之后，将每个二叉树中的左子节点都加入相应的队列；

     - 如果两个原始二叉树的左子节点有一个为空，即有一个原始二叉树的左子树为空，则合并后的二叉树的左子树即为另一个原始二叉树的左子树，此时也不需要对非空左子树继续遍历，因此不需要将左子节点加入队列。

    对于右子节点和右子树，处理方法与左子节点和左子树相同。