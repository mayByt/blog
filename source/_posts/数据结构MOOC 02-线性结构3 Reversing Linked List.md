---
title: 数据结构MOOC 02-线性结构3 Reversing Linked List
date: 2021-08-14 21:12:37
updated: 2021-08-14 21:12:37
tags: 数据结构
categories: 数据结构
description:
---

# **02-线性结构 3 Reversing Linked List**

## 题目要求

Given a constant *K* and a singly linked list *L*, you are supposed to reverse the links of every *K* elements on *L*. For example, given *L* being 1→2→3→4→5→6, if *K*=3, then you must output 3→2→1→6→5→4; if *K*=4, you must output 4→3→2→1→5→6.

### Input Specification:

Each input file contains one test case. For each case, the first line contains the address of the first node, a positive *N* (≤10<sup>5</sup>) which is the total number of nodes, and a positive *K* (≤*N*) which is the length of the sublist to be reversed. The address of a node is a 5-digit nonnegative integer, and NULL is represented by -1.

Then *N* lines follow, each describes a node in the format:

```
Address Data Next
```

where `Address` is the position of the node, `Data` is an integer, and `Next` is the position of the next node.

### Output Specification:

For each case, output the resulting ordered linked list. Each node occupies a line, and is printed in the same format as in the input.

### Sample Input:

```in
00100 6 4
00000 4 99999
00100 1 12309
68237 6 -1
33218 3 00000
99999 5 68237
12309 2 33218
```

### Sample Output:

```out
00000 4 33218
33218 3 12309
12309 2 00100
00100 1 99999
99999 5 68237
68237 6 -1
```

### Solution:

题目大致要求是做一个链表的部分反转，要求按照`Address Data Next`的格式进行输入，其中 Address 就是当前节点的位置，Data 是节点存储的数据，Next 是下一节点的位置。第一行输入的是链表头结点的位置以及节点的总数还有反转子链的长度 K。最后也按照`Address Data Next`的格式输出，但是是按照部分反转后的顺序输出。

根据题目要求搭建程序框架：

```
int main()
{
	输入链表
	链表部分反转操作
	输出结果链表
}
```

因此，大体上需要设计输入，反转，输出三个功能的函数。

① 链表的输入函数

首先观察题目的输入样例，发现数据是用`Address` 和 `Next` 来实现链表功能，因此可以用结构体数组来存储数据，实现链表功能。

```cpp
#define MAXLEN 100005

struct Node {
    int Data;   // 数据
    int Next;   // 下一节点地址下标
};
typedef struct Node Link;
Link LinkList[MAXLEN];
```

按照题目要求，要按照`Address Data Next` 的格式输入节点数据，而第一行输入略有不同，第一行输入的是链表头结点的位置以及节点的总数还有部分翻转的计数 K。根据以上要求，设计链表的输入函数。

```cpp
// 输入函数，返回链表头结点位置下标
int ReadList(Link &L)
{
    int Head, n, k; // 起始节点的位置下标，节点总数，反转子链的长度
    int address, num, next;
    cin >> Head >> n >> k;
    while(n--)
    {
        cin >> address >> num >> next;
        L[address].Data = num;
        L[address].Next = next;
    }

    return Head;
}
```

② 链表的反转操作

该部分是解题的关键，也是难点所在。

注意这里有一个坑，测试点中有多余节点不在链表上的情况，因此添加一个计数函数，统计链表有效节点，排除不在链表上的离散干扰节点的影响。

```cpp
// 计数函数，统计链表有效节点，排除不在链表上的离散干扰节点的影响
int count(Link L[], int head)
{
    int cnt = 1;
    for(int i = head; L[i].Next != -1; i = L[i].Next)
        cnt++;

    return  cnt;

}
```

执行反转的操作关键在于将下一个结点的 Next 变为上一个节点，但是这样又会导致丢失下一个节点中原来 Next 的位置，因此需要三个指针来完成反转操作。前两个指针用来进行反转，第三个指针临时存储下一节点位置。同时由于反转子链后需要连接下一段子链，因此需要记录上一段子链最后一个节点的位置以及下一段子链第一个节点的位置。

这里也有小坑，注意考虑 K=1 的情况，此时不需要反转。

```cpp
// 链表反转函数
void ReverseList(Link L[], int &head, int k)  // 执行反转操作的链表，链表的头结点位置下标，反转子链的长度
{
    if(k == 1)
        return;
    int cnt = count(L,head);    // 链表有效长度
    int p1, p2, p3; // p1 和 p2 用来执行反转操作，p3 临时存储下一节点位置
    int nextHead = head;   // 标记下一段反转子链的第一个结点
    int lastEnd = -2;   // 标记上一段反转子链的最后一个节点，因为-1 是链表结束标志，因此赋初值 wei-2
    bool first = 1; // 标记第一条反转子链，需要将反转后的第一个子链的头结点赋值为整个链表的头结点
    while(cnt >= k)
    {
        p1 = nextHead;
        p2 = L[p1].Next;
        for(int i=1; i<k; i++)
        {
            p3 = L[p2].Next;    // 将 p2 的下一个结点位置存储
            L[p2].Next = p1;    // 反转 p1 与 p2 的前后顺序，将 p2 的下一个结点指向 p1
            p1 = p2;    // 向后遍历，指针后移
            p2 = p3;
        }
        L[nextHead].Next = p3;  // 链接反转子链与下一段子链
        if(first)    // 如果是第一条反转子链
        {
            head = p1;  // 将反转后的第一个子链的头结点赋值为整个链表的头结点
            lastEnd = nextHead; // 反转子链之前的第一个结点变成最后一个节点
        }

        else
        {
            L[lastEnd].Next = p1;   // 开始新的反转子链
            lastEnd = nextHead;
        }
        nextHead = p2;
        first = 0;
        cnt -= k;
    }
}
```

③ 打印链表的函数

因为输出限制了格式需要将地址按照 5 位输出，并且空位补零，因此使用 C 语言的 prinf() 函数控制格式，“%05d"控制 5 位补零。但是由于最后一个结点的 Next 为-1，因此需要另外单独控制输出。

```cpp
// 打印链表函数
void PrintList(Link L[], int head)
{
    int i = head;
    while(L[i].Next != -1)
    {
        printf("%05d %d %05d\n", i, L[i].Data, L[i].Next);
        i = L[i].Next;
    }
    printf("%05d %d %d\n",i, L[i].Data, L[i].Next);
}
```



### Code

```cpp
#include<bits/stdc++.h>
using namespace std;

#define MAXLEN 100005

struct Node {
    int Data;   // 数据
    int Next;   // 下一节点地址下标
};
typedef struct Node Link;
Link LinkList[MAXLEN];  // 结构体数组表示链表
int k, head;    // 反转子链长度，链表头

int ReadList(Link[]);    // 输入函数
void ReverseList(Link L[], int &head, int k); // 链表反转函数
void PrintList(Link L[], int head);   // 打印链表

int main()
{
    head = ReadList(LinkList);
    ReverseList(LinkList, head, k);
    PrintList(LinkList,head);

    return 0;
}

// 输入函数，返回链表头结点位置下标
int ReadList(Link L[])
{
    int Head, n; // 起始节点的位置下标，节点总数，反转子链的长度
    int address, num, next;
    cin >> Head >> n >> k;
    while(n--)
    {
        cin >> address >> num >> next;
        L[address].Data = num;
        L[address].Next = next;
    }

    return Head;
}
// 计数函数，统计链表有效节点，排除不在链表上的离散干扰节点的影响
int count(Link L[], int head)
{
    int cnt = 1;
    for(int i = head; L[i].Next != -1; i = L[i].Next)
        cnt++;

    return  cnt;

}
// 链表反转函数
void ReverseList(Link L[], int &head, int k)  // 执行反转操作的链表，链表的头结点位置下标，反转子链的长度
{
    if(k == 1)
        return;
    int cnt = count(L,head);    // 链表有效长度
    int p1, p2, p3; // p1 和 p2 用来执行反转操作，p3 临时存储下一节点位置
    int nextHead = head;   // 标记下一段反转子链的第一个结点
    int lastEnd = -2;   // 标记上一段反转子链的最后一个节点，因为-1 是链表结束标志，因此赋初值 wei-2
    bool first = 1; // 标记第一条反转子链，需要将反转后的第一个子链的头结点赋值为整个链表的头结点
    while(cnt >= k)
    {
        p1 = nextHead;
        p2 = L[p1].Next;
        for(int i=1; i<k; i++)
        {
            p3 = L[p2].Next;    // 将 p2 的下一个结点位置存储
            L[p2].Next = p1;    // 反转 p1 与 p2 的前后顺序，将 p2 的下一个结点指向 p1
            p1 = p2;    // 向后遍历，指针后移
            p2 = p3;
        }
        L[nextHead].Next = p3;  // 链接反转子链与下一段子链
        if(first)    // 如果是第一条反转子链
        {
            head = p1;  // 将反转后的第一个子链的头结点赋值为整个链表的头结点
            lastEnd = nextHead; // 反转子链之前的第一个结点变成最后一个节点
        }

        else
        {
            L[lastEnd].Next = p1;   // 开始新的反转子链
            lastEnd = nextHead;
        }
        nextHead = p2;
        first = 0;
        cnt -= k;
    }
}
// 打印链表函数
void PrintList(Link L[], int head)
{
    int i = head;
    while(L[i].Next != -1)
    {
        printf("%05d %d %05d\n", i, L[i].Data, L[i].Next);
        i = L[i].Next;
    }
    printf("%05d %d %d\n",i, L[i].Data, L[i].Next);
}
```

---

