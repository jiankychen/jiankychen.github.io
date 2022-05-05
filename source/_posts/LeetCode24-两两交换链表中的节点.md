---
title: LeetCode 24. 两两交换链表中的节点
tags:
  - 链表
  - 双指针
categories:
  - LeetCode
cover: false
abbrlink: f84c47a9
date: 2022-04-29 10:16:01
---

[LeetCode 24. Swap Nodes in Pairs](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

 

**Example 1:**


    Input: head = [1,2,3,4]
    Output: [2,1,4,3]


**Example 2:**

    Input: head = []
    Output: []


**Example 3:**

    Input: head = [1]
    Output: [1]
 

**Constraints:**

 - The number of nodes in the list is in the range `[0, 100]`.
 - $0 \le$ `Node.val` $\le 100$

## 思路

**一定要先画图模拟**

下面以交换 `cur` 与 `latter1` 为例，示意交换两个链表节点的过程：

1. 初始时：
    ```mermaid
    graph LR;
        pre --> cur;
        cur --> latter1;
        latter1 --> latter2;
    ```

2. 令 `pre` 的 `next` 指针指向 `latter1`
    ```mermaid
    graph LR;
        pre -.- cur;
        pre --> latter1;
        cur --> latter1;
        latter1 --> latter2;
    ```

3. 保存指向 `latter2` 的指针 `temp`，并且，令 `latter1` 的 `next` 指针指向 `cur`
    ```mermaid
    graph LR;
        pre --> latter1;
        cur --> latter1;
        latter1 --> cur;
        latter1 -.- latter2;
    ```

4. 令 `cur` 的 `next` 指针指向 `latter2`
    ```mermaid
    graph LR;
        pre --> latter1;
        latter1 --> cur;
        cur -.- latter1;
        cur --> latter2;
    ```

5. 通过以上步骤，实现节点 `cur` 与节点 `latter1` 的交换，所得链表为：
    ```mermaid
    graph LR;
        pre --> latter1;
        latter1 --> cur;
        cur --> latter2;
    ```

## Method: 双指针

维护 `pre` 指针和 `cur` 指针，依照上述步骤完成 `cur` 和 `cur` 下个节点 `cur->next` 的交换

遍历 `cur` 指针，直到 `cur` 为空 或 `cur->next` 为空
 - 若 `cur` 为空，则链表节点数为偶数，此前的两两交换刚好完成所有节点交换
 - 若 `cur` 不为空但 `cur->next` 为空，则链表节点数为奇数，两两交换后还剩最后一个节点，此时无需再进行交换

代码实现：

```cpp
// 省略了结构体 ListNode 的定义
ListNode* swapPairs(ListNode* head) {
    ListNode* dummyHead = new ListNode(); // 设置虚拟头节点，以便交换 head 节点及其下个节点
    dummyHead->next = head;
    ListNode* cur = head;                 // 当前遍历节点
    ListNode* pre = dummyHead;            // 当前遍历节点 cur 的上个节点
    while (cur != nullptr && cur->next != nullptr) { // 交换 cur 及其下个节点
        // 拷贝指向 cur 下下个节点的指针（因为后续会修改 cur->next->next）
        ListNode* temp = cur->next->next;
        // 令 pre 的 next 指针指向 cur 的下个节点
        pre->next = cur->next;
        // 令 cur 下个节点的 next 指针指向 cur
        cur->next->next = cur;
        // 令 cur 的 next 指针指向 temp
        cur->next = temp;
        // pre 与 cur 同时向后移动
        pre = cur;
        cur = cur->next;
    }
    return dummyHead->next;               // 返回 dummyHead->next
}
```

时间复杂度：$O(n)$

空间复杂度：$O(1)$