---
title: 数据结构MOOC 02-线性结构2 一元多项式的乘法与加法运算
date: 2021-08-14 11:06:25
updated: 2021-08-14 11:06:25
tags: 数据结构
categories: 数据结构
description:
---

# **02-线性结构2 一元多项式的乘法与加法运算**

## 题目要求

设计函数分别求两个一元多项式的乘积与和。

### 输入格式:

输入分2行，每行分别先给出多项式非零项的个数，再以指数递降方式输入一个多项式非零项系数和指数（绝对值均为不超过1000的整数）。数字间以空格分隔。

### 输出格式:

输出分2行，分别以指数递降方式输出乘积多项式以及和多项式非零项的系数和指数。数字间以空格分隔，但结尾不能有多余空格。零多项式应输出`0 0`。

### 输入样例:

```in
4 3 4 -5 2  6 1  -2 0
3 5 20  -7 4  3 1
```

### 输出样例:

```out
15 24 -25 22 30 21 -10 20 -21 8 35 6 -33 5 14 4 -15 3 18 2 -6 1
5 20 -4 4 -5 2 9 1 -2 0
```

### 解题思路：

根据输入样例，多项式均是以指数递减的方式输入，可用动态数组和单链表来表示多项式，本题采用单链表来表示。链表结构如下

```cpp
typedef struct PolyNode *Polynomial;   // 多项式
struct PolyNode {
    int coef;   // 系数
    int expon;  // 指数
    Polynomial link;
}
```

按照题目要求搭建程序框架：

```
int main()
{
	读取多项式1
	读取多项式2
	乘法运算并输出
	加法运算并输出
	
	return 0;
}
```

因此，需要设计读取，加法和乘法运算，输出四个功能的函数。

① 读取函数

根据题目的输入样例，设计读取函数，由于链表中结点链接到链表中从操作频繁，增加设计一个链接函数Attach()，方便操作。

```cpp
// 链接函数
void Attach(int c, int e, Polynomial *pRear)
{
    Polynomial t = new Polynomial();
    t->coef = c;    // 给新节点赋值
    t->expon = e;
    t->link = NULL;
    (*pRear)->link = t;
    *pReal = t; // 修改*pRear的值
}
// 读取多项式函数
Polynomial ReadPoly()
{
    Polynomial P = new Polynomial();  // 临时生成链表头空节点
    Polynomial Rear, t;
    P->link = NULL;
    Rear = P;
    int n, c, e;
    cin >> n;
    while(n--)
    {
        cin >> c >> e;
        Attach(c,e,&Rear);
    }
    t = P;  // 删除临时生成的头节点
    P = P->link;
    free(t);
    return P;
}
```

② 多项式相加函数

两个多项式相加，就是逐项比较两个链表每一项，将指数相同的项系数相加得到新的项，指数较大的项链接到结果链表后并将指针向后移，直到其中一个链表遍历完，将另一链表剩余部分全部连接到结果链表后。

```cpp
// 多项式相加函数
Polynomial AddPoly(Polynomial P1, Polynomial P2)
{

    Polynomial Rear, t1, t2, t;
    t1 = P1;
    t2 = P2;
    Polynomial P = new PolyNode;
    P->link = NULL;
    Rear = P;
    while(t1 && t2)
    {
        if(t1->expon == t2->expon)
        {
            int sum = t1->coef + t2->coef;
            if(sum != 0)
            {
                Attach(sum,t1->expon,&Rear);
                t1 = t1->link;
                t2 = t2->link;
            }
            else
            {
                t1 = t1->link;
                t2 = t2->link;
            }

        }
        else if(t1->expon > t2->expon)
        {
            Attach(t1->coef,t1->expon,&Rear);
            t1 = t1->link;
        }
        else if(t1->expon < t2->expon)
        {
            Attach(t2->coef,t2->expon,&Rear);
            t2 = t2->link;
        }
    }
    for(;t1;t1=t1->link)    Attach(t1->coef,t1->expon,&Rear);
    for(;t2;t2=t2->link)    Attach(t2->coef,t2->expon,&Rear);
    Rear->link = NULL;
    t = P;
    P = P->link;
    free(t);

    return P;
}
```

③ 多项式相乘函数

两个多项式相乘较为麻烦，可以分解为两个主要步骤：

​	<1> 将乘法运算转变为加法运算

​			将P1当前项（c<sub>i</sub> ,e<sub>i</sub> ) 乘 P2多项式，再加到结果多项式里

```cpp
t1 = P1; t2 = P2;
Polynomial P = new Polynomial();
Rear = P;
while(t2)
{
	Attach(t1->coef*t2->coef,t1->expon+t2->expon,&Rear);
	t2 = t2->link;
}
```

​	<2> 逐项插入

​			将P1当前项（c1<sub>i</sub> ,e1<sub>i</sub> ) 乘P2当前项（c2<sub>i</sub> ,e2<sub>i</sub> )，并插入到结果多项式中。关键是要找到插入位置。其中结果多项式的初始位置可由P1的第一项乘以P2获得。 

```cpp
t1 = t1->link;
    while(t1)
    {
        t2 = P2;
        Rear = P;   // 每一次的t1指向的多项式乘以t2的每一项后，Rear都重新指回P的头结点，以便下一次得到的t1和t2的乘积插入P
        while(t2)
        {
            c = t1->coef * t2->coef;
            e = t1->expon + t2->expon;
            while (Rear->link && Rear->link->expon > e) // 当Rear的下一项不为空且下一项的指数比当前的乘积e大时，Rear后移
            {
                Rear = Rear->link;
            }
            if(Rear->link && Rear->link->expon == e)
            {
                if(Rear->link->coef+c)  // 指数相同时，结果系数不为零
                {
                    Rear->link->coef+=c;
                }
                else    // 指数相同时，结果系数为0
                {
                    t = Rear->link; // 将零项删除
                    Rear->link = t->link;
                    free(t);
                }
            }
            else    // 当前多项式的指数大于Rear的下一项
            {
                t = new PolyNode;
                t->coef = c;
                t->expon = e;
                t->link = Rear->link;
                Rear->link = t;
                Rear = Rear->link;
            }
            t2 = t2->link;
        }
        t1 = t1->link;
    }
```

④ 输出函数

​	要按照输出样例的格式输出，注意到输出的多项式每组系数和指数间都有空格，每个项之间也有空格，但是首尾没有空格。

​	可以把每组数据看作，“空格+系数+指数”，但是第一项没有空格，因此可以设置一个flag来控制输出，flag 初始值为0，在执行一步输出操作后置为1，只有flag为1时才输出空格。

```cpp
// 打印结果多项式函数
void PrintPoly(Polynomial P)
{
    bool flag = 0;
    if(!P)
    {
        cout << "0 0" << endl;
    }
    else
    {
        while(P)
        {
            if(!flag)
                flag = 1;
            else
                cout << " ";
            cout << P->coef << " " << P->expon;
            P = P->link;
        }
    }
    cout << endl;
}
```



### 代码：

```cpp
#include<bits/stdc++.h>
using namespace std;

typedef struct PolyNode *Polynomial;   // 多项式
struct PolyNode {
    int coef;   // 系数
    int expon;  // 指数
    Polynomial link;
};
void Attach(int c, int e, Polynomial *pRear);  // 链接结点到链表的函数
Polynomial ReadPoly(); // 读取多项式函数
Polynomial AddPoly(Polynomial P1, Polynomial P2);    // 多项式相加
Polynomial MultPoly(Polynomial P1, Polynomial P2);  // 多项式相乘
void PrintPoly(Polynomial P);   // 打印结果多项式函数

int main()
{
    Polynomial P1, P2, Pa, Pm;
    P1 = ReadPoly();
    P2 = ReadPoly();
    Pa = AddPoly(P1,P2);
    Pm = MultPoly(P1,P2);
    PrintPoly(Pm);
    PrintPoly(Pa);

    return 0;
}

// 链接函数
void Attach(int c, int e, Polynomial *pRear)
{
    Polynomial t = new PolyNode;
    t->coef = c;    // 给新节点赋值
    t->expon = e;
    t->link = NULL;
    (*pRear)->link = t;
    *pRear = t; // 修改*pRear的值
}
// 读取多项式函数
Polynomial ReadPoly()
{
    Polynomial P = new PolyNode;  // 临时生成链表头空节点
    Polynomial Rear, t;
    P->link = NULL;
    Rear = P;
    int n, c, e;
    cin >> n;
    while(n--)
    {
        cin >> c >> e;
        Attach(c,e,&Rear);
    }
    t = P;  // 删除临时生成的头节点
    P = P->link;
    free(t);

    return P;
}
// 多项式相加函数
Polynomial AddPoly(Polynomial P1, Polynomial P2)
{

    Polynomial Rear, t1, t2, t;
    t1 = P1;
    t2 = P2;
    Polynomial P = new PolyNode;
    P->link = NULL;
    Rear = P;
    while(t1 && t2)
    {
        if(t1->expon == t2->expon)
        {
            int sum = t1->coef + t2->coef;
            if(sum != 0)
            {
                Attach(sum,t1->expon,&Rear);
                t1 = t1->link;
                t2 = t2->link;
            }
            else
            {
                t1 = t1->link;
                t2 = t2->link;
            }

        }
        else if(t1->expon > t2->expon)
        {
            Attach(t1->coef,t1->expon,&Rear);
            t1 = t1->link;
        }
        else if(t1->expon < t2->expon)
        {
            Attach(t2->coef,t2->expon,&Rear);
            t2 = t2->link;
        }
    }
    for(;t1;t1=t1->link)    Attach(t1->coef,t1->expon,&Rear);
    for(;t2;t2=t2->link)    Attach(t2->coef,t2->expon,&Rear);
    Rear->link = NULL;
    t = P;
    P = P->link;
    free(t);

    return P;
}
// 多项式相乘函数
Polynomial MultPoly(Polynomial P1, Polynomial P2)
{
    int c, e;
    if(!P1 || !P2)  return NULL;    // 判断两个多项式都不为零多项式
    Polynomial t, t1, t2, Rear;
    t1 = P1;    t2 = P2;
    Polynomial P = new PolyNode;
    P->link = NULL;
    Rear = P;
    while(t2)   // 先用P1的第一项乘以P2作为结果多项式的初始值
    {
        Attach(t1->coef*t2->coef,t1->expon+t2->expon,&Rear);
        t2 = t2->link;
    }
    t1 = t1->link;
    while(t1)
    {
        t2 = P2;
        Rear = P;   // 每一次的t1指向的多项式乘以t2的每一项后，Rear都重新指回P的头结点，以便下一次得到的t1和t2的乘积插入P
        while(t2)
        {
            c = t1->coef * t2->coef;
            e = t1->expon + t2->expon;
            while (Rear->link && Rear->link->expon > e) // 当Rear的下一项不为空且下一项的指数比当前的乘积e大时，Rear后移
            {
                Rear = Rear->link;
            }
            if(Rear->link && Rear->link->expon == e)
            {
                if(Rear->link->coef+c)  // 指数相同时，结果系数不为零
                {
                    Rear->link->coef+=c;
                }
                else    // 指数相同时，结果系数为0
                {
                    t = Rear->link; // 将零项删除
                    Rear->link = t->link;
                    free(t);
                }
            }
            else    // 当前多项式的指数大于Rear的下一项
            {
                t = new PolyNode;
                t->coef = c;
                t->expon = e;
                t->link = Rear->link;
                Rear->link = t;
                Rear = Rear->link;
            }
            t2 = t2->link;
        }
        t1 = t1->link;
    }
    t2 = P;
    P = P->link;
    free(t2);

    return P;
}
// 打印结果多项式函数
void PrintPoly(Polynomial P)
{
    bool flag = 0;
    if(!P)  // 注意判断P是否为0多项式
    {
        cout << "0 0";
    }
    else
    {
        while(P)
        {
            if(!flag)
                flag = 1;
            else
                cout << " ";
            cout << P->coef << " " << P->expon;
            P = P->link;
        }
    }
    cout << endl;
}
```