---
title: LeetCode 225. 用队列实现栈
tags:
  - 栈
  - 队列
categories:
  - LeetCode
cover: false
abbrlink: 5a1b1370
date: 2022-05-19 10:30:53
---

[LeetCode 225. Implement Stack using Queues](https://leetcode.cn/problems/implement-stack-using-queues/)

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

`void push(int x)` Pushes element `x` to the top of the stack.
`int pop()` Removes the element on the top of the stack and returns it.
`int top()` Returns the element on the top of the stack.
`boolean empty()` Returns `true` if the stack is empty, `false` otherwise.


**Notes:**

 - You must use **only** standard operations of a queue, which means that only `push to back`, `peek/pop from front`, `size` and `is empty` operations are valid.
 - Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.
 

**Example 1:**

    Input
    ["MyStack", "push", "push", "top", "pop", "empty"]
    [[], [1], [2], [], [], []]
    Output
    [null, null, null, 2, 2, false]

    Explanation
    MyStack myStack = new MyStack();
    myStack.push(1);
    myStack.push(2);
    myStack.top(); // return 2
    myStack.pop(); // return 2
    myStack.empty(); // return False
 

**Constraints:**

 - $1 \le$ x $\le 9$
 - At most 100 calls will be made to `push`, `pop`, `top`, and `empty`.
 - All the calls to `pop` and `top` are valid.
 

**Follow-up:** Can you implement the stack using only one queue?

## 思路

栈的特点是 后进先出 ，队列的特点是 先进先出

可定义一个队列，使得 最先入栈的元素 放在 队首 ，最后入栈的元素 放在 队尾

 - 对于入栈操作，只需将元素添加到队尾

 - 对于出栈操作，需将队尾元素移除并返回

 - 为获取栈顶元素，可先将栈顶元素出栈，并记录其值（本题定义的 出栈 函数具有返回值），然后再将其压入栈
 - 判断栈是否为空，只需判断用来存储栈元素的队列是否为空

## Method: 两个队列

定义队列 `que1` ，用于存储 栈 的数据（ 最先入栈的元素 放在 队首 ，最后入栈的元素 放在 队尾 ）

定义队列 `que2` ，在 出栈 操作中，用于临时存放队列 `que1` 的前 `que1.size() - 1` 个元素，以便将 `que1` 队尾元素移除

> 事实上，由于队列具有先进先出的特性，无需额外定义一个队列 `que2` 来实现 出栈 过程中的临时存放，而是直接将元素重新入队 `que1` 即可，这样就能使得最初的队尾元素出现在队首


## Method: 一个队列

[^_^]: 被注释掉了

    算法流程：

    1. 定义队列 `que` ，存储栈元素：最先入栈的元素 放在 队首 ，最后入栈的元素 放在 队尾

    2. `void push(int x)` 函数的实现：将元素添加到 `que` 队尾

    3. `int pop()` 函数的实现：将 `que` 的前 `que.size() - 1` 个元素依次 弹出 并 添加到队尾，使得栈顶元素出现在队首，记录其值后移除，返回其值即可

    4. `int top()` 函数的实现：调用 `int pop()` 函数移除栈顶元素并获取其值，然后调用 `void push(int x)` 函数将其重新压入栈，最后返回栈顶元素值

    5. `boolean empty()` 函数的实现：判断 `que` 是否为空即可



代码实现：

```cpp
class MyStack {
public:
    queue<int> que; // 最先入栈的放在 que 队首，最后入栈的放在 que 队尾

    MyStack() {

    }
    
    void push(int x) {
        que.push(x);
    }
    
    int pop() {
        int length = que.size(); // 记录 que1 的长度，注意，不能直接将 que.size() 作为 for 循环的判定条件
        // 将 que 前 length - 1 个元素依次移出，然后放入队尾，使得最初的队尾元素出现在队首
        for (int i = 0; i < length - 1; i++) {
            int temp = que.front();
            que.pop();
            que.push(temp);
        }
        // 记录最初的队尾元素值，并将其移除
        int ans = que.front();
        que.pop();
        return ans;
    }
    
    int top() {
        int ans = pop(); // 复用已经定义的 MyStack 类别的 pop() 函数，将栈顶元素弹出
        push(ans);       // 再将其压入栈
        return ans;
    }
    
    bool empty() {
        return que.empty() ? true : false;
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```