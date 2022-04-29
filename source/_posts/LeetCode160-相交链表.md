---
title: LeetCode160-相交链表
date: 2022-04-29 16:42:45
tags:
 - 链表
categories:
 - LeetCode
cover: false
---

Given the heads of two singly linked-lists `headA` and `headB`, return *the node at which the two lists intersect*. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:

```mermaid
graph LR;
    1((a1)) --> 2((a2)) --> 3((c1)) --> 4((c2)) --> 5((c3));
    6((b1)) --> 7((b1)) --> 8((b3)) --> 3((c1));
```

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

**Custom Judge:**

The inputs to the **judge** are given as follows (your program is not given these inputs):

 - `intersectVal` - The value of the node where the intersection occurs. This is `0` if there is no intersected node.
 - `listA` - The first linked list.
 - `listB` - The second linked list.
 - `skipA` - The number of nodes to skip ahead in listA (starting from the head) to get to the intersected node.
 - `skipB` - The number of nodes to skip ahead in listB (starting from the head) to get to the intersected node.

The judge will then create the linked structure based on these inputs and pass the two heads, `headA` and `headB` to your program. If you correctly return the intersected node, then your solution will be **accepted**.


**Example 1:**

```mermaid
graph LR;
    1((4)) --> 2((1)) --> 3((8)) --> 4((4)) --> 5((5));
    6((5)) --> 7((6)) --> 8((1)) --> 3((8));
```

    Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3

    Output: Intersected at '8'

    Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
    From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.


**Example 2:**

```mermaid
graph LR;
    1((1)) --> 2((9)) --> 3((1)) --> 4((2)) --> 5((4));
    6((3)) --> 4((2));
```

    Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1

    Output: Intersected at '2'

    Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
    From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.


**Example 3:**

```mermaid
graph LR;
    1((2)) --> 2((6)) --> 3((4));
    4((1)) --> 5((5));
```

    Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2

    Output: No intersection

    Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
    Explanation: The two lists do not intersect, so return null.
 

**Constraints:**

 - The number of nodes of `listA` is in the `m`.
 - The number of nodes of `listB` is in the `n`.
 - 1 <= `m`, `n` <= 3 * 10^4^
 - 1 <= `Node.val` <= 10^5^
 - 0 <= `skipA` < m
 - 0 <= `skipB` < n
 - `intersectVal` is `0` if `listA` and `listB` do not intersect.
 - `intersectVal == listA[skipA] == listB[skipB]` if `listA` and `listB` intersect.
 

**Follow up:** Could you write a solution that runs in `O(m + n)` time and use only `O(1)` memory?
