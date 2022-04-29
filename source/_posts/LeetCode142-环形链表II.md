---
title: LeetCode 142. 环形链表 II
date: 2022-04-29 19:52:07
tags:
 - 链表
 - 双指针
categories:
 - LeetCode
cover: false
---

[LeetCode 142. Linked List Cycle II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

Given the `head` of a linked list, return *the node where the cycle begins*. If there is no cycle, return `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. Note that `pos` is **not passed as a parameter**.

**Do not modify** the linked list.

 
**Example 1:**

![Example 1](LeetCode142-环形链表II/1.png)

    Input: head = [3,2,0,-4], pos = 1
    Output: tail connects to node index 1
    Explanation: There is a cycle in the linked list, where tail connects to the second node.


**Example 2:**

![Example 2](LeetCode142-环形链表II/2.png)

    Input: head = [1,2], pos = 0
    Output: tail connects to node index 0
    Explanation: There is a cycle in the linked list, where tail connects to the first node.

**Example 3:**

![Example 3](LeetCode142-环形链表II/3.png)

    Input: head = [1], pos = -1
    Output: no cycle
    Explanation: There is no cycle in the linked list.
 

**Constraints:**

 - The number of the nodes in the list is in the range `[0, 104]`.
 - -10^5^ <= `Node.val` <= 10^5^
 - `pos` is `-1` or a valid index in the linked-list.
 

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?


## 思路

### 判断是否有环

定义 `fast` 和 `slow` 指针，从头结点出发，`fast` 指针每次移动两个节点，`slow` 指针每次移动一个节点，如果 `fast` 和 `slow` 指针在途中相遇 ，说明这个链表有环

 - 若 `fast` 与 `slow` 相遇，则一定有环：因为 `fast` 超前 `slow` ，相遇时二者一定都在环内

 - 若链表有环，则 `fast` 与 `slow` 一定相遇：当 `slow` 步入到环内时，由于 `fast` 指针每次移动相对于 `slow` 指针而言都是移动一位，故而一定会相遇

如下图所示：

![](https://tva1.sinaimg.cn/large/008eGmZEly1goo4xglk9yg30fs0b6u0x.gif)


### 确定环的入口



## 快慢指针