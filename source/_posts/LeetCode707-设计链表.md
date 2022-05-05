---
title: LeetCode 707. 设计链表
date: 2022-04-28 20:26:37
tags: 
 - 链表
categories:
 - LeetCode
cover: false
---

[LeetCode 707. Design Linked List](https://leetcode-cn.com/problems/design-linked-list/)

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.
A node in a singly linked list should have two attributes: `val` and `next`. `val` is the value of the current node, and `next` is a pointer/reference to the next node.
If you want to use the doubly linked list, you will need one more attribute prev to indicate the previous node in the linked list. Assume all nodes in the linked list are **0-indexed**.

Implement the MyLinkedList class:

 - `MyLinkedList()` Initializes the `MyLinkedList` object.
 - `int get(int index)` Get the value of the `index`-th node in the linked list. If the `index` is invalid, return `-1`.
 - `void addAtHead(int val)` Add a node of value `val` before the **first** element of the linked list. After the insertion, the new node will be the first node of the linked list.
 - `void addAtTail(int val)` Append a node of value `val` as the **last** element of the linked list.
 - `void addAtIndex(int index, int val)` Add a node of value `val` **before** the `index`-th node in the linked list. If `index` equals the length of the linked list, the node will be appended to the end of the linked list. If `index` is greater than the length, the node will not be inserted.
 - `void deleteAtIndex(int index)` Delete the `index`-th node in the linked list, if the `index` is valid.
 

**Example 1:**

    Input: 
    ["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
    [[], [1], [3], [1, 2], [1], [1], [1]]
    Output: 
    [null, null, null, null, 2, null, 3]

    Explanation
    MyLinkedList myLinkedList = new MyLinkedList();
    myLinkedList.addAtHead(1);
    myLinkedList.addAtTail(3);
    myLinkedList.addAtIndex(1, 2);    // linked list becomes 1->2->3
    myLinkedList.get(1);              // return 2
    myLinkedList.deleteAtIndex(1);    // now the linked list is 1->3
    myLinkedList.get(1);              // return 3
 

**Constraints:**

 - $0 \le$ `index`, `val` $\le 1000$
 - Please do not use the built-in LinkedList library.
 - At most `2000` calls will be made to `get`, `addAtHead`, `addAtTail`, `addAtIndex` and `deleteAtIndex`.


## 思路

旨在设计链表的五个接口，以实现链表的常见操作：
 - 获取链表第 `index` 个节点的数值
 - 在链表的最前面插入一个节点
 - 在链表的最后面插入一个节点
 - 在链表第 `index` 个节点前面插入一个节点
 - 删除链表的第 `index` 个节点

链表操作的两种方式：
 - 直接使用原来的链表来进行操作
 - 设置一个虚拟头结点再进行操作



## Method: 单链表

接下来以设计单链表的五个接口为例，采用设置虚拟头节点的方式执行相关操作

```cpp
// 定义链表的结构体
struct LinkedNode {
    int val;
    LinkedNode *next;
    LinkedNode() : val(0), next(nullptr) {}
    LinkedNode(int x) : val(x), next(nullptr) {}
};

class MyLinkedList {

    LinkedNode* dummyHead = new LinkedNode();
    int size;

public:

    // 初始化链表
    MyLinkedList() {
        dummyHead = new LinkedNode(0);    // 虚拟头节点
        size = 0;
    }
    
    // 获取链表第 index 个节点的值
    int get(int index) {
        if (index < 0 || index > (size - 1))
            return -1;
        LinkedNode* cur = dummyHead->next;
        while (index) { // 循环结束时 cur 指向第 index 个节点
            cur = cur->next;
            index--;
        }
        return cur->val;
    }
    
    // 在链表的第一个元素之前插入值为 val 的节点，注意要更新链表长度
    void addAtHead(int val) {
        LinkedNode* newHead = new LinkedNode(val);
        newHead->next = dummyHead->next;
        dummyHead->next = newHead;
        size++;
    }
    
    // 在链表的末尾添加值为 val 的节点，注意要更新链表长度
    void addAtTail(int val) {
        LinkedNode* cur = dummyHead;
        while (cur->next != nullptr)
            cur = cur->next;
        LinkedNode* newTail = new LinkedNode(val);
        cur->next = newTail;
        size++;
    }
    
    // 在链表中的第 index 个节点之前添加值为 val 的节点，注意要更新链表长度
    void addAtIndex(int index, int val) {
        if (index > size)
            return;
        LinkedNode* cur = dummyHead;
        while (index) { // 循环结束时 cur 指向第 index - 1 个节点
            cur = cur->next;
            index--;
        }
        LinkedNode* newNode = new LinkedNode(val);
        newNode->next = cur->next;
        cur->next = newNode;
        size++;
    }

    // 删除第 index 个节点，注意要更新链表长度
    void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        LinkedNode* cur = dummyHead;
        while(index) { // 循环结束时 cur 指向第 index - 1 个节点
            cur = cur->next;
            index--;
        }
        LinkedNode* tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;
        size--;
    }
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```
## Method: 双链表