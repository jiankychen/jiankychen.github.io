---
title: LeetCode 150. 逆波兰表达式求值
tags:
  - 栈
  - 字符串
categories:
  - LeetCode
cover: false
abbrlink: 7936534f
date: 2022-05-20 10:17:45
---

[LeetCode 150. Evaluate Reverse Polish Notation](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

Evaluate the value of an arithmetic expression in *Reverse Polish Notation*.

Valid operators are `+`, `-`, `*`, and `/`. Each operand may be an integer or another expression.

Note that *division between two integers should truncate toward zero*.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

 

**Example 1:**

    Input: tokens = ["2","1","+","3","*"]
    Output: 9
    Explanation: ((2 + 1) * 3) = 9


**Example 2:**

    Input: tokens = ["4","13","5","/","+"]
    Output: 6
    Explanation: (4 + (13 / 5)) = 6


**Example 3:**

    Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
    Output: 22
    Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
    = ((10 * (6 / (12 * -11))) + 17) + 5
    = ((10 * (6 / -132)) + 17) + 5
    = ((10 * 0) + 17) + 5
    = (0 + 17) + 5
    = 17 + 5
    = 22
 

**Constraints:**

 - $1 \le$ `tokens.length` $\le 10^4$
 - `tokens[i]` is either an operator: `"+"`, `"-"`, `"*"`, or `"/"`, or an integer in the range `[-200, 200]`.


## 思路

逆波兰表达式，即，**后缀表达式**（运算符写在两个操作数的后面）

日常使用的是中缀表达式（运算符写在两个操作数的中间）

例如，`2 + 3` 就是中缀表达式，对应的后缀表达式是 `2 3 +`

由此，本题的求解思路为：

1. 定义一个 栈

2. 遍历数组 `tokens` ，若遇到的是数字，则将数字压入栈，若遇到的是 `"+"` 、`"-"` 、`"*"` 、`"/"` 运算符，则从栈内取出两个数字进行计算，然后将计算结果压入栈中

3. 遍历结束时，栈内剩余元素即为整个后缀表达式的计算结果

注意，数组 `tokens` 内的元素是 `string` 类型的对象，**将 `string` 型的数字压入栈前需将其转换成 `int` 型**
 - 调用 `stoi` 函数即可，参考 [cplusplus：std::stoi](http://www.cplusplus.com/reference/string/stoi/?kw=stoi)


## Method: 栈

代码实现：

```cpp
// 计算后缀表达式，并将结果压入栈
void compute(stack<int> &stk, string c) {
    int right = stk.top(); // 右操作数
    stk.pop();
    int left = stk.top();  // 左操作数
    stk.pop();
    if (c == "+") stk.push(left + right);
    else if (c == "-") stk.push(left - right);
    else if (c == "*") stk.push(left * right);
    else if (c == "/") stk.push(left / right);
}

int evalRPN(vector<string>& tokens) {
    stack<int> stk;
    for (auto c : tokens) {
        if (c == "+" || c == "-" || c == "*" || c == "/")
            compute(stk, c);
        else
            stk.push(stoi(c)); // 将 string 类型转成 int 型
    }
    int ans = stk.top();
    stk.pop();
    return ans;
}
```

时间复杂度：$O(n)$，其中，$n$ 为 `tokens` 的长度

空间复杂度：$O(n)$，栈所需空间