---
title: 数据结构MOOC课后习题 01-复杂度3 二分查找 （函数题）
date: 2021-08-12 23:31:43
updated: 2021-08-12 23:31:43
tags: 数据结构
categories: 数据结构
description:
---

# **01-复杂度 3 二分查找** （函数题）

## 题目要求

本题要求实现二分查找算法。

### 函数接口定义：

```cpp
Position BinarySearch( List L, ElementType X );
```

其中`List`结构定义如下：

```cpp
typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};
```

`L`是用户传入的一个线性表，其中`ElementType`元素可以通过>、==、<进行比较，并且题目保证传入的数据是递增有序的。函数`BinarySearch`要查找`X`在`Data`中的位置，即数组下标（注意：元素从下标 1 开始存储）。找到则返回下标，否则返回一个特殊的失败标记`NotFound`。

### 裁判测试程序样例：

```cpp
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 10
#define NotFound 0
typedef int ElementType;

typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};

List ReadInput(); /* 裁判实现，细节不表。元素从下标 1 开始存储 */
Position BinarySearch( List L, ElementType X );

int main()
{
    List L;
    ElementType X;
    Position P;

    L = ReadInput();
    scanf("%d", &X);
    P = BinarySearch( L, X );
    printf("%d\n", P);

    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例 1：

```
5
12 31 55 89 101
31
```

### 输出样例 1：

```
2
```

### 输入样例 2：

```
3
26 78 233
31
```

### 输出样例 2：

```
0
```

### 代码：

```cpp
Position BinarySearch( List L, ElementType X )
{
    Position t = 0, l = 1, r = L->Last;
    while(l <= r)
    {
        t = (l+r) / 2;
        if(X == L->Data[t])
        {
            return t;
        }
        else if(X > L->Data[t])
        {
            l = t + 1;
        }
        else if(X < L->Data[t])
        {
            r = t - 1;
        }
    }
    return NotFound;
}
```

### 解题思路：

简单的二分思想，设置一个 temp 不断寻找待查区间下标的中间值，与输入的数据比较，如果 temp 较大，则更新待查区间右端点为 temp-1；若 temp 较小，则更新待查区间左端点为 temp+1；若与 temp 相等，则返回 temp，即该元素在线性表中的位置下标；若出现了待查区间左端点下标大于右端点下标的情况，则说明未在线性表中查找到该元素，返回 NotFound。