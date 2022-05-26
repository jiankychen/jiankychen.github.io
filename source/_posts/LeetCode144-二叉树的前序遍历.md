---
title: LeetCode 144. 二叉树的前序遍历
tags:
 - 二叉树
categories:
 - LeetCode
math: true
abbrlink: 1e967892
date: 2022-05-26 15:41:55
---

[LeetCode 144. Binary Tree Preorder Traversal](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

Given the `root` of a binary tree, return *the preorder traversal of its nodes' values.*


**Example 1:**
![](LeetCode144-二叉树的前序遍历/1.jpg)

    Input：root = [1,null,2,3]
    Output：[1,2,3]

**Example 2:**
![](LeetCode144-二叉树的前序遍历/2.jpg)

    Input：root = [1,2]
    Output：[1,2]

**Example 3:**
![](LeetCode144-二叉树的前序遍历/3.jpg)

    Input：root = [1,null,2]
    Output：[1,2]

**Constraints:**

 - The number of nodes in the tree is in the range `[0, 100]`.
 - $-100 \le$ `Node.val` $\le 100$
 

**Follow up:** Recursive solution is trivial, could you do it iteratively?



## Method 1: 递归


算法思路：

按照访问根节点——左子树——右子树的方式遍历这棵树，而在访问左子树或者右子树的时候，我们按照同样的方式遍历，直到遍历完整棵树。

定义 `preorder(root)` 表示当前遍历到 `root` 节点的答案。按照定义，我们只要首先将 `root` 节点的值加入答案，然后递归调用 `preorder(root.left)` 来遍历 `root` 节点的左子树，最后递归调用 `preorder(root.right)` 来遍历 `root` 节点的右子树即可，递归终止的条件为碰到`空节点`。

代码实现：

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    preorder(root,res);         // 先序遍历，调用递归函数
    return res;
}

void preorder(TreeNode* root, vector<int> &res) {
    if (root == nullptr) return;
    res.push_back(root->val);   // 记录遍历的顺序
    preorder(root->left,res);   // 左子树
    preorder(root->right,res);  // 右子树
}
```

时间复杂度：$O(n)$，其中 $n$ 是二叉树的节点数。每一个节点恰好被遍历一次

空间复杂度：$O(n)$，为递归过程中栈的开销，平均情况下为 $O(\log{n})$，最坏情况下树呈现链状，为 $O(n)$


## Method 2: 迭代


算法思路：

设计一个栈，用于存放将要被访问的树的根节点，利用栈的后进先出特性实现前序遍历

首先将把根节点入栈，然后重复以下过程，直到栈为空
 - 从栈中取出一个节点，输出节点的值
 - 把右子节点放入栈
 - 把左子节点放入栈

代码实现：

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    if (root == nullptr)
        return res;

    stack<TreeNode*> stk;
    TreeNode* node = root;
    while (!stk.empty() || node != nullptr) {
        while (node != nullptr) {
            res.emplace_back(node->val);
            stk.emplace(node);
            node = node->left;
        }
        node = stk.pop();
        node = node->right;
    }
    return res;
}
```

时间复杂度：$O(n)$，其中 $n$ 是二叉树的节点数。每一个节点恰好被遍历一次

空间复杂度：$O(n)$，为迭代过程中显式栈的开销，平均情况下为 $O(\log{n})$，最坏情况下树呈现链状，为 $O(n)$



[^_^]: 这部分被注释掉了
    ## Method 3: Morris遍历
    J. H. Morris 在 1979 年的论文「Traversing Binary Trees Simply and Cheaply」中首次提出

    Morris 遍历的核心思想是利用树的大量空闲指针，实现空间开销的极限缩减。其前序遍历规则总结如下：
    1. 新建临时节点，令该节点为 `root`；
    2. 如果当前节点的左子节点为空，将当前节点加入答案，并遍历当前节点的右子节点；
    3. 如果当前节点的左子节点不为空，在当前节点的左子树中找到当前节点在中序遍历下的前驱节点：
        - 如果前驱节点的右子节点为空，将前驱节点的右子节点设置为当前节点。然后将当前节点加入答案，并将前驱节点的右子节点更新为当前节点。当前节点更新为当前节点的左子节点。
        - 如果前驱节点的右子节点为当前节点，将它的右子节点重新设为空。当前节点更新为当前节点的右子节点。

    4. 重复步骤 2 和步骤 3，直到遍历结束。

    ```cpp
    vector<int> preorderTraversal(TreeNode *root) {
        vector<int> res;
        if (root == nullptr) {
            return res;
        }

        TreeNode *p1 = root, *p2 = nullptr;

        while (p1 != nullptr) {
            p2 = p1->left;
            if (p2 != nullptr) {
                while (p2->right != nullptr && p2->right != p1) {
                    p2 = p2->right;
                }
                if (p2->right == nullptr) {
                    res.emplace_back(p1->val);
                    p2->right = p1;
                    p1 = p1->left;
                    continue;
                } else {
                    p2->right = nullptr;
                }
            } else {
                res.emplace_back(p1->val);
            }
            p1 = p1->right;
        }
        return res;
    }
    ```

    时间复杂度：$O(n)$，其中 $n$ 是二叉树的节点数。没有左子树的节点只被访问一次，有左子树的节点被访问两次

    空间复杂度：$O(1)$，只操作已经存在的指针（树的空闲指针），因此只需要常数的额外空间


[力扣官方题解：二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/er-cha-shu-de-qian-xu-bian-li-by-leetcode-solution/)