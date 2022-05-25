---
title: 命名空间的using声明
categories:
  - C++
cover: false
abbrlink: 65b35a95
date: 2022-03-03 09:40:55
tags:
---

`std::cin`的意思就是要使用命名空间`std`中的名字`cin`。

`using`声明具有如下的形式：
```cpp
using namespace::name;
```
一旦声明了上述语句，就可以直接访问命名空间中的名字。
```cpp
#include <iostream>
// using declaration; when we use the name cin, we get the one from the namespace std
using std::cin;
int main()
{
    int i;
    cin >> i;       // ok: cin is a synonym for std::cin
    cout << i;      // error: no using declaration; we must use the full name
    std::cout << i; // ok: explicitly use cout from namepsace std
    return 0;
}
```

# 每个名字都需要独立的using声明
按照规定，每个`using`声明引入命名空间中的一个成员。因此，用到的每个名字都必须有自己的声明语句，而且**每句话都得以分号结束。**
```cpp
#include <iostream>
// using declarations for names from the standard library
using std::cin;
using std::cout;
using std::endl;
int main()
{
    cout << "Enter two numbers:" << endl;
    int v1, v2;
    cin >> v1 >> v2;
    cout << "The sum of " << v1 << " and " << v2
        << " is " << v1 + v2 << endl;
    return 0;
}
```

# 头文件不应包含using声明
位于头文件的代码一般来说不应该使用`using`声明。
> 因为头文件的内容会拷贝到所有引用它的文件中去，如果头文件里有某个`using`声明，那么每个使用了该头文件的文件就都会有这个声明。对于某些程序来说，由于不经意间包含了一些名字，反而可能产生始料未及的名字冲突。