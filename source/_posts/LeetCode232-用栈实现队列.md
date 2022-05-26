---
title: LeetCode 232. 用栈实现队列
tags:
  - 栈
  - 队列
categories:
  - LeetCode
cover: false
abbrlink: 81d08953
date: 2022-05-18 22:29:08
---


[LeetCode 232. Implement Queue using Stacks](https://leetcode.cn/problems/implement-queue-using-stacks/)

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

 - `void push(int x)` : Pushes element `x` to the back of the queue.
 - `int pop()` : Removes the element from the front of the queue and returns it.
 - `int peek()` : Returns the element at the front of the queue.
 - `boolean empty()` : Returns `true` if the queue is empty, `false` otherwise.

**Notes:**

 - You must use **only** standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
 - Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.
 

**Example 1:**

    Input
    ["MyQueue", "push", "push", "peek", "pop", "empty"]
    [[], [1], [2], [], [], []]
    Output
    [null, null, null, 1, 1, false]

    Explanation
    MyQueue myQueue = new MyQueue();
    myQueue.push(1); // queue is: [1]
    myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
    myQueue.peek(); // return 1
    myQueue.pop(); // return 1, queue is [2]
    myQueue.empty(); // return false
 

**Constraints:**

 - $1 \le$ `x` $\le 9$
 - At most `100` calls will be made to `push`, `pop`, `peek`, and `empty`.
 - All the calls to `pop` and `peek` are valid.
 

**Follow-up:** Can you implement the queue such that each operation is *amortized* `O(1)` time complexity? In other words, performing n operations will take overall `O(n)` time even if one of those operations may take longer.


## 思路

栈具有后进先出的特性

若将一个栈的所有元素移入到另一个栈中，元素的排列顺序会变得与之前相反

## Method

解题思路：

1. 定义两个栈，`stk1` 和 `stk2`
    - 栈 `stk1` 用来作为存储队列，即，最先入队的在 `stk1` 栈顶，最后入队的在 `stk1` 栈底
    - 栈 `stk2` 作为临时存储空间，用来协助实现队列的功能

2. `void push(int x)` 函数的实现：
    - 若 `stk1` 非空：先将 `stk1` 内的所有元素都移出来，放进 `stk2` ，然后将 `x` 压入栈 `stk1` ，将 `stk2` 内的元素移回 `stk1`
    - 若 `stk1` 为空，直接将 `x` 压入栈 `stk1` 即可

3. `int pop()` 函数的实现：
    - 记录 `stk1` 栈顶元素值，并弹出 `stk1` 栈顶元素，返回其值

4. `int peek()` 函数的实现：
    - 直接返回 `stk1` 栈顶元素值即可

5. `bool empty()` 函数的实现：
    - 判断 `stk1` 是否为空即可

> 另，也可以 “将最先入队的放 `stk1` 栈底，最后入队的在 `stk1` 栈顶” ，此时：入队操作可直接将 `x` 压入栈 `stk1` ；出队操作需将 `stk1` 栈底以上的元素全都临时存放到 `stk2` ，待 `stk1` 栈底元素弹出后，再将 `stk2` 所有元素移回 `stk1`

代码实现：

```cpp
class MyQueue {
public:

    stack<int> stk1;
    stack<int> stk2;

    MyQueue() {

    }
    
    void push(int x) {
        if (!stk1.empty()) { // stk1 非空
            while (!stk1.empty()) { // 将 stk1 元素压入栈 stk2
                stk2.push(stk1.top());
                stk1.pop();
            }
            stk1.push(x);    // 将 x 压入栈 stk1
            while (!stk2.empty()) { // 将 stk2 元素放回栈 stk1
                stk1.push(stk2.top());
                stk2.pop();
            }
        }
        else  // stk1 为空，直接将 x 压入栈 stk1
            stk1.push(x);
    }
    
    int pop() {
        int ans = stk1.top();
        stk1.pop();
        return ans;
    }
    
    int peek() {
        int ans = stk1.top();
        return ans;
    }
    
    bool empty() {
        if (stk1.empty()) return true;
        return false;
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
 ```

注意，标准库实现的栈和队列，其成员函数 `pop()` 没有返回值，不能作为右值；`top()` 具有返回值，可以作为右值

另外，关于 `peek()` 函数的实现，可以直接调用已经定义了的 `MyQueue` 类别的 `pop()` 函数，即，通过函数的复用来实现。这样可以降低出错的可能性