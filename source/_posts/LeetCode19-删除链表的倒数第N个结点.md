---
title: LeetCode 19. 删除链表的倒数第N个结点
date: 2022-03-16 22:26:43
tags:
 - 双指针
 - 链表
categories:
 - LeetCode
cover: false
---

[LeetCode](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

Given the head of a linked list, remove the nth node from the end of the list and return its head.

 

Example 1:

![](LeetCode19-删除链表的倒数第N个结点/1.png)

    Input: head = [1,2,3,4,5], n = 2
    Output: [1,2,3,5]



Example 2:

    Input: head = [1], n = 1
    Output: []


Example 3:

    Input: head = [1,2], n = 1
    Output: [1]


## Method: 快慢指针

> 在对链表进行操作时，一种常用的技巧是添加一个**哑节点**（dummy node），它的 `next` 指针指向链表的头节点。这样一来，我们就不需要对头节点进行特殊的判断了。
> 
> 在本题中，**如果我们要删除节点 `y`，我们需要知道节点 `y` 的前驱节点 `x`，并将 `x` 的指针指向 `y` 的后继节点**，即，**`x->next = x->next->next`**。
> 
> 但由于头节点不存在前驱节点，因此我们需要在删除头节点时进行特殊判断。但如果我们添加了哑节点，那么头节点的前驱节点就是哑节点本身，此时我们就只需要考虑通用的情况即可。

由于我们需要找到倒数第 `n` 个节点，因此我们可以使用两个指针 `fast` 和 `slow` 同时对链表进行遍历，并且 `fast` 比 `slow` 超前 `n` 个节点。当 `fast` 遍历到链表的末尾时，`slow` 就恰好处于倒数第 `n` 个节点。

如果我们能够得到的是倒数第 `n` 个节点的前驱节点，删除操作会更加方便。因此我们可以考虑在初始时将 `slow` 指向`哑节点`，其余的操作步骤不变。这样一来，当 `fast` 遍历到链表的末尾时，`slow` 的下一个节点就是我们需要删除的节点。只需要修改一次指针，`slow->next = slow->next->next`，就能完成删除操作。

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode *dummy = new ListNode(0, head); // 创建一个哑结点，记录链表开头，同时用来实现head=[1]等特殊情况的结点删除
    ListNode *fast = head, *slow = dummy;
    for (int num = 0; num < n; num++)
        fast = fast->next;
    while (fast != nullptr)
    {
        fast = fast->next;
        slow = slow->next;
    }
    slow->next = slow->next->next;  // 把需要删除的结点的前一个和后一个连接，从而删除结点
    ListNode *ans = dummy->next;
    delete dummy;
    return ans;
}
```


时间复杂度：`O(L)`，其中 `L` 是链表的长度。

空间复杂度：`O(1)`。



## Method: 栈

## Method: 计算链表长度