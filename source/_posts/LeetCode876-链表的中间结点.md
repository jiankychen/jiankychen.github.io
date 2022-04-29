---
title: LeetCode 876. 链表的中间结点
date: 2022-03-16 21:54:52
tags: 
 - 链表
 - 双指针
categories:
 - LeetCode
cover: false
---

[LeetCode](https://leetcode-cn.com/problems/middle-of-the-linked-list/)


Given the head of a singly linked list, return `the middle node` of the linked list.

If there are two middle nodes, return `the second middle` node.

Example 1:

    Input: head = [1,2,3,4,5]
    Output: [3,4,5]
    Explanation: The middle node of the list is node 3.


Example 2:

    Input: head = [1,2,3,4,5,6]
    Output: [4,5,6]
    Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.


## Method 1: 数组
对链表进行遍历，同时将遍历到的元素依次放入数组`A`中。如果我们遍历到了`N`个元素，那么链表以及数组的长度也为`N`，对应的中间节点即为`A[N/2]`

```cpp
ListNode* middleNode(ListNode* head) {
    vector<ListNode*> A = {head};
    while (A.back()->next != NULL)
        A.push_back(A.back()->next);
    return A[A.size() / 2];
}
```

时间复杂度：`O(N)`，其中`N`是给定链表中的结点数目。

空间复杂度：`O(N)`，即数组`A`用去的空间。

## Method 2: 单指针
对链表进行两次遍历。第一次遍历时，我们统计链表中的元素个数`N`；第二次遍历时，我们遍历到第`N/2`个元素（链表的首节点为第`0`个元素）时，将该元素返回即可

> 注意，题目要求「两个中间结点的时候，返回**第二个中间结点**」。此时，快指针可以前进的条件是：**当前快指针和当前快指针的下一个结点都非空**。
>  
> 如果题目要求「在两个中间结点的时候，返回**第一个中间结点**」，快指针可以前进的条件是：**当前快指针的下一个结点和当前快指针的下下一个结点都非空**。
```cpp
ListNode* middleNode(ListNode* head) {
    ListNode *current = head;
    int n = 0;
    while (current != nullptr)      // 统计链表的节点数
    {
        n++;
        current = current->next;
    }
    current = head;
    for (int i = 0; i < n / 2; i++) // 寻找链表的中间节点
        current = current->next;
    return current;
}
```

时间复杂度：`O(N)`，其中`N`是给定链表的结点数目。

空间复杂度：`O(1)`，只需要常数空间存放变量和指针。


## Method 3: 快慢指针
用两个指针`slow`与`fast`一起遍历链表。`slow`一次走一步，`fast`一次走两步。那么当`fast`到达链表的末尾时，`slow`必然位于中间

```cpp
ListNode* middleNode(ListNode* head) {
    // 快慢指针，快指针一次前进两步，慢指针一次前进一步
    ListNode *fast = head, *slow = head;
    // 判断条件为：快指针当前节点非空且下一节点都非空。这样才能保证返回第二个中间节点（存在两个中间节点时）
    while (fast != nullptr && fast->next != nullptr)    
    {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

时间复杂度：`O(N)`，其中N是给定链表的结点数目。

空间复杂度：`O(1)`，只需要常数空间存放`slow`和`fast`两个指针。


[题解：链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/solution/lian-biao-de-zhong-jian-jie-dian-by-leetcode-solut/)

[题解：快慢指针（注意链表长度为偶数时，返回第 2 个结点的细节）](https://leetcode-cn.com/problems/middle-of-the-linked-list/solution/kuai-man-zhi-zhen-zhu-yao-zai-yu-diao-shi-by-liwei/)