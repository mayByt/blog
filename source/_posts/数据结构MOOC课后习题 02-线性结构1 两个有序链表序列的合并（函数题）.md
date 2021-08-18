---
title: 数据结构MOOC课后习题 02-线性结构1 两个有序链表序列的合并（函数题）
date: 2021-08-13 11:19:13
updated: 2021-08-13 11:19:13
tags: 数据结构
categories: 数据结构
description:
---

# **02-线性结构1 两个有序链表序列的合并**（函数题）

## 题目要求

本题要求实现一个函数，将两个链表表示的递增整数序列合并为一个非递减的整数序列。

### 函数接口定义：

```c++
List Merge( List L1, List L2 );
```

其中`List`结构定义如下：

```c++
typedef struct Node *PtrToNode;
struct Node {
    ElementType Data; /* 存储结点数据 */
    PtrToNode   Next; /* 指向下一个结点的指针 */
};
typedef PtrToNode List; /* 定义单链表类型 */
```

`L1`和`L2`是给定的带头结点的单链表，其结点存储的数据是递增有序的；函数`Merge`要将`L1`和`L2`合并为一个非递减的整数序列。应直接使用原序列中的结点，返回归并后的带头结点的链表头指针。

### 裁判测试程序样例：

```cpp
#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;
typedef struct Node *PtrToNode;
struct Node {
    ElementType Data;
    PtrToNode   Next;
};
typedef PtrToNode List;

List Read(); /* 细节在此不表 */
void Print( List L ); /* 细节在此不表；空链表将输出NULL */

List Merge( List L1, List L2 );

int main()
{
    List L1, L2, L;
    L1 = Read();
    L2 = Read();
    L = Merge(L1, L2);
    Print(L);
    Print(L1);
    Print(L2);
    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例：

```in
3
1 3 5
5
2 4 6 8 10
```

### 输出样例：

```out
1 2 3 4 5 6 8 10 
NULL
NULL
```

### 代码：

```cpp
List Merge( List L1, List L2 )
{
    List L = (List)malloc(sizeof(struct Node));
    List tempL1 = L1->Next, tempL2 = L2->Next;
    List head = L;  // 合并后链表L的头结点
    while(tempL1 && tempL2)
    {
        if(tempL1->Data <= tempL2->Data)
        {
            L->Next = tempL1;
            tempL1 = tempL1->Next;
            L = L->Next;
        }
        else
        {
            L->Next = tempL2;
            tempL2 = tempL2->Next;
            L = L->Next;
        }
    }
    L->Next = tempL1 ? tempL1 : tempL2; // 若一个链表遍历完成，则将另一个链表剩余部分直接接到合并后链表
    L1->Next = NULL;
    L2->Next = NULL;
    return head;
}
```

### 解题思路

合并链表的操作重点就是将较小链表节点（此处记为tempL）插入到合并链表之后：

```cpp
1. L->Next = tempL;
2. tempL = tempL->Next;
3. L = L->Next;
```

直到一个链表遍历完，此时将另一链表剩余部分全部接到合并链表尾：

```cpp
L->Next = tempL1 ? tempL1 : tempL2;
```

在题目的输出样例中，L1 和 L2打印均为 NULL，因此，在函数最后将 L1->Next 和 L2->Next 全部置为NULL。

