---
title: 优先级队列
tags:
  - 优先级队列
categories:
  - 数据结构
cover: false
abbrlink: a21107fc
date: 2022-04-30 16:22:32
---

队列是一个先进先出的线性表

优先级队列：结点之间的关系是由结点的优先级决定的，而不是由入队的先后次序决定。优先级高的先出队，优先级低的后出队

优先级队列可以基于线性结构实现
 - 按照 **优先级** 排序
   - 入队：按照优先级插入在合适的位置 - $O(N)$
   - 出队：队头元素 - $O(1)$
 - 按照 **到达时间** 排序
   - 入队：插入到队尾 - $O(1)$
   - 出队：寻找优先级最高的并删除 - $O(N)$


优先级队列也可以基于 **二叉堆** 实现

 - 二叉堆指的是父子之间的大小满足一定约束的完全二叉树

   - 最小化堆：父结点的值小于等于儿子结点
   - 最大化堆：父结点的值大于等于儿子结点

 - 出队：删除根结点 - $O(\log{N})$

 - 入队：在最后一层的第一个空位置上添加一个元素，但添加后要调整元素的位置，以保持堆的有序性 - $O(\log{N})$

> 在优先队列运行的过程中，其本身结构一直为完全二叉树

## 优先级队列类

数据成员设计
 - 采用顺序存储

数据成员：
 - 队列长度
 - 指向数组起始地址的指针
 - 数组大小

成员函数：
 - 抽象类规定的所有功能
 - 创建一个队列：`create()`
 - 入队：`enQueue(x)`
 - 出队：`deQueue()`
 - 读队头：`getHead`
 - 判队空：`isEmpty`

私有的工具函数：
 - 扩展数组的工具函数 `doubleSpace`
 - 构造时的工具函数 `buildHeap`
 - 出队时的工具函数 `percolateDown`

类定义：

```cpp
template <class Type>
class priorityQueue:public queue<Type> {
    private:
        // 队列长度
        int currentSize;  
        // 指向数组起始地址的指针
        Type *array; 
        // 数组大小
        int maxSize;
        void doubleSpace();
        void buildHeap();
        void percolateDown(int hole);  
    public:
        priorityQueue(int capacity = 100);
        priorityQueue(const Type data[], int size);
        ~priorityQueue() { delete [] array; }
        bool isEmpty() const { return currentSize == 0; }
        void enQueue(const Type & x);
        Type deQueue();
        Type getHead() { return array[1]; }
};

// 构造函数 - 构造空的优先级队列
priorityQueue(int capacity = 100) { 
    array = new Type[capacity];
    maxSize = capacity;   
    currentSize = 0;
}
```


## 入队与出队操作

基于二叉堆实现的优先级队列

### 入队 `enque`

即，在堆中插入一个新元素（以最小化堆为例）

操作要求
 - 保证结构性：插入在最底层的第一个空位
 - 保证有序性：向上移动到合适的位置（向上过滤）

实现：
```cpp
template <class Type>
void priorityQueue<Type>::enQueue( const Type & x) {
    if(currentSize == maxSize - 1) 
        doubleSpace();

    int hole = ++currentSize;
    for(; hole > 1 && x < array[hole / 2]; hole /= 2)
        array[hole] = array[hole / 2]; // 向上过滤（父节点下移）
    array[hole] = x;
} 
```

时间复杂度：
 - 最坏情况是 $O(\log{n})$，其中 $n$ 为元素个数，$\log{n}$ 为完全二叉树的高度
 - 有资料表明，向上过滤平均进行 2.6 次比较，因此元素平均上移 1.6 层

### 出队 `deque`

删除根节点（以最小化堆为例）

操作要求：
 - 保证结构性：将最后一个节点移到根节点
 - 保证有序性：向下移动到合适的位置（向下过滤）
   - 从两个子节点中找出值较小的子节点，将其向上移

实现：
```cpp
template <class Type>
Type priorityQueue<Type>::deQueue() { 
    Type minItem;
    minItem = array[1];
    array[1] = array[currentSize--];
    // 从 1 号下标开始向下过滤
    percolateDown(1);
    return minItem;
} 

template <class Type>
void priorityQueue<Type>::percolateDown(int hole) { // 向下过滤，hole 为过滤起点
    int child;
    Type tmp = array[hole];

    for(; hole * 2 <= currentSize; hole = child) { 
        child = hole * 2;
        if (child != currentSize && array[child + 1] < array[child])
            child++; // child 对应值较小的子节点的下标
        if(array[child] < tmp) // 违反有序性，向下过滤
            array[hole] = array[child];
        else break;
    }
    array[hole] = tmp;
}
```

时间复杂度：最坏和平均都是 $O(\log{n})$，即，完全二叉树的高度
 - 根据堆的有序性，堆中最后一个节点的值一般比较大，因此，向下过滤很少提前结束


## 堆的构建

将一组元素组织成一个堆

方法一：在空堆中，执行 $N$ 次连续插入
 - 复杂度：最坏是 $O(N \log{N})$，平均是 $O(N)$
 - 每一次插入之后都要维护堆的有序性，会做很多额外的工作

方法二：不考虑中间状态，保证所有元素加入后满足堆的特性
 - 做法：利用递归将左子树和右子树调整成堆，然后对根结点调用 `percolateDown` ，以恢复堆的有序性
 - 复杂度： 最坏情况下是 $O(N)$

因此，采用方法二建堆

定义 `buildHeap` 函数维护堆的有序性，其代码实现如下：
```cpp
// 以逆向层次的顺序对节点调用 percolateDown
template <class Type>
void priorityQueue<Type>::buildHeap() { // 调用此前定义的 percolateDown 函数向下建堆
    for (int i = currentSize / 2; i > 0; i--) // 按逆向层次的顺序建堆，从 currentSize 的父节点开始
        percolateDown(i);
}
```

带初值的构造函数
 - 申请存储堆的空间
 - 将参数数组存入
 - 调用 `buildHeap` 建堆

实现：
```cpp
// 带初值的构造函数
template <class Type>
priorityQueue<Type>::priorityQueue(const Type *items, int size)
                   : maxSize(size + 10 ), currentSize(size) {
    array = new Type[maxSize];
    for(int i = 0; i < size; i++) // 将元素加入堆
        array[i + 1] = items[i]; // 优先级队列从 1 开始索引
    buildHeap(); // 维护堆的有序性
}
```

建堆性能分析：
 - 高度为 $h$ 的节点，在 `percolateDown` 中交换的最大次数是 $h - 1$ 。建堆的最坏时间是所有节点的调整时所需交换次数之和，即所有节点的高度之和
 -  对于一棵高度为 $h$ ，包含了 $N = 2^h - 1$ 个结点的满二叉树，向下过滤中交换次数和的最大值为 $N - h$


## 基于二项堆的优先级队列

支持归并的优先级队列

1. 左堆

2. 斜堆

3. 二项堆
     - 又被称为贝努里队列，是一连串的贝努里树的集合，且每个高度的树至多出现一次
     - 结构性：高度为 0 的二项树是单个节点，高度为 $k$ 的二项树 $B_k$ 是将一棵 $B_{k-1}$ 加到另一棵 $B_{k-1}$ 的根上
     - 有序性：满足堆的有序性

基于二项堆的优先级队列
 - 二项树出现与否，正好对应元素数目的二进制表示。如，13 可表示为 1101 ，其中，二进制第 $i$ 位的值表示 二项树 $B_i$ 是否出现，则 13 个元素的优先级队列由 $B_3$、$B_2$ 和 $B_0$ 组成

基于二项堆的优先级队列非常方便归并操作

 - 归并操作：由低到高依次归并两个优先级队列中高度相同的树——将根结点大的作为根结点小的树的子树。如果由于前一次归并而出现三棵高度相同的树时，留下一棵，归并其余两棵。最坏时间复杂度是 $O(\log{N})$

 - 插入操作：插入是归并的特例——将被插入结点形成一棵单结点的树组成的集合，归并两个集合。最坏时间复杂度是 $O(logN)$ 。平均是 2 次操作即可
 - 删除操作：找出具有最小根值的树 T ，先将 T 从原先的集合中删掉，再在 T 中删除根结点，最后归并 T 与原先的集合。最坏时间复杂度是 $O(\log{N})$


## STL中的优先级队列
头文件：`<queue>`
类模板：`priority_queue`