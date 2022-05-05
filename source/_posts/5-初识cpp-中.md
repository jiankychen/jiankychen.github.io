---
title: 初识C++(中)
categories:
  - C++
cover: false
abbrlink: bf7c72c5
date: 2021-10-27 12:48:55
tags:
---

# while
while 语句的形式是
```cpp
while (condition)
    statement
```
`while`语句持续地交替检测`condition`和执行`statement`，直至`condition`为假为止

# 语句块
语句块(block)，是指用花括号`{}`包围的零条或多条语句的序列

语句块也是语句的一种，在任何要求使用语句的地方都可以使用语句块，如：

```cpp
int sum = 0, val = 1;
while (val <= 10)
    {
        sum = sum + val;        // 等价于 sum += val;
        val = val + 1;          // 等价于 ++ val;
    }
```

# for
`for`语句包含循环头和循环体，循环头控制循环体的执行次数

循环头：初始化语句(init-statement)、循环条件(condition)、表达式(expression)，即：
`for (init-statement; condition; expression)`

**初始化语句只在`for`循环入口处执行一次。在初始化语句中定义的变量，在循环结束之后是不能使用的**

循环体每次执行前都会先检查循环条件

表达式在`for`循环体之后执行（若循环体不被执行，则表达式亦不会被执行）

执行完表达式后，`for`语句重新检测循环条件，循环持续至循环条件为假
```cpp
int sum = 0;
for (int val = 1; val <= 10; ++ val)
{
    sum += val;
}
```
例中，初始化语句中定义的变量`val`在`for`语句结束后即失效


# for与while

## for
一般用于循环次数已知的情况，并且`for`循环可以节省内存以及代码简洁，在初始化语句中定义一个局部变量，循环结束后，局部变量即被释放

## while
一般用于循环次数不确定的情况，并且通常需要在循环之前定义变量



# 读取数量不定的输入数据
```cpp
#include <iostream>
int main()
{
    int sum = 0, value = 0;
    // 读取数据直至遇到文件尾，计算所有读入的值的和
    while (std::cin >> value)
        sum += value;
    std::cout << "Sum is: " << sum << std::endl;
    return 0;
}
```
例中，数据读取操作是在`while`的循环条件中完成的

`while`循环条件的求值就是执行表达式`std::cin >> value`，表达式`std::cin >> value`返回输入运算符`>>`左侧运算对象，即`std::cin`，因此，该循环条件实际上是检测`std::cin`

当我们使用一个`istream`对象如`cin`作为条件时，其效果是检测流(stream)的状态，当遇到文件结束符(end-of-file)或遇到一个无效的输入时(例如读入的值不是一个整数)，`istream`对象的状态会变为无效。进一步地，无效状态的`istream`对象会使条件变为假，循环结束

键盘输入文件结束符：`Ctrl + Z`然后按`Enter`或`Return`键(windows系统)，`Ctrl + D`键(UNIX系统、Mac OS X系统)


# if
```cpp
if (condition)
{
    statement;
}
```
条件`condition`为真，执行条件之后的语句块`statement`，否则不予执行

或者
```cpp
if (condition)
{
    statement1;
}
else
{
    statement2;
}
```
条件`condition`为真时执行`statement1`，否则执行`statement2`


示例：用`if`语句统计输入序列中每个值连续出现了多少次
```cpp
#include <iostream>
using std::cin;
using std::cout;
using std::endl;
int main()
{
    int currentNum, num;            // currentNum为当前统计的数字，num为新读取的数字
    if (cin >> currentNum)          // 初始化当前统计数字
    {
        int count = 1;              // 初始化当前统计数字的出现次数
        while (cin >> num)          // 读取下一个数字
        {
            if (num == currentNum)  // 如果新输入数字与当前统计的数字相同，出现次数加一
                ++ count;
            else                    // 如果不同，输出当前统计数字的出现次数，并更新所统计的数字，重置计数器
            {
                cout << currentNum << " occurs " << count << " times " << endl;
                currentNum = num;
                count = 1;
            }
        }
        cout << currentNum << " occurs " << count << " times " << endl;     // 输出文件最后一个数字的出现次数
    }
    return 0;
}
```