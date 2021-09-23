---
title: 数据结构MOOC 02-线性结构4 Pop Sequence
date: 2021-08-15 14:39:31
updated: 2021-08-15 14:39:31
tags: 数据结构
categories: 数据结构
description:
---

# **02-线性结构 4 Pop Sequence**

## 题目要求

Given a stack which can keep *M* numbers at most. Push *N* numbers in the order of 1, 2, 3, ..., *N* and pop randomly. You are supposed to tell if a given sequence of numbers is a possible pop sequence of the stack. For example, if *M* is 5 and *N* is 7, we can obtain 1, 2, 3, 4, 5, 6, 7 from the stack, but not 3, 2, 1, 7, 5, 6, 4.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 3 numbers (all no more than 1000): *M* (the maximum capacity of the stack), *N* (the length of push sequence), and *K* (the number of pop sequences to be checked). Then *K* lines follow, each contains a pop sequence of *N* numbers. All the numbers in a line are separated by a space.

### Output Specification:

For each pop sequence, print in one line "YES" if it is indeed a possible pop sequence of the stack, or "NO" if not.

### Sample Input:

```
5 7 5
1 2 3 4 5 6 7
3 2 1 7 5 6 4
7 6 5 4 3 2 1
5 6 4 3 7 2 1
1 7 6 5 4 3 2
```

### Sample Output:

```
YES
NO
NO
YES
NO
```

### Solution：

题目要求是按照 1, 2, 3, ..., *N* 的顺序将数字压入栈中，但是会随机在某一时刻弹出，要求我们可以判断给出的出栈序列是否符合栈的入栈出栈规范，是否可能实现，是的话输出 “YES”，否输出“NO”。

解决方法主要就是模拟入栈和出栈的过程，在此过程中一旦发现违反栈 “先进后出” 的规则（栈顶元素大于当前弹出元素），或者栈溢出，就证明该种情况不成立；若将给出的测试序列全部走完未发现问题，则证明该种情况可能实现。

程序第一行要求输入三个数据：M（栈的最大容量）、N（每个输入序列的数据个数）、K（待测试的序列个数），接下来输入 K 行待检测序列，开始模拟入栈出栈操作。

### Code：

```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int m, n, k;    // 堆栈最大容量，序列元素个数，待检测序列个数
    cin >> m >> n >> k;
    int stack[1005] = {0};    // 数组做堆栈，但是这里的数组大小不是堆栈的最大容量
    int top = -1;   // 栈顶指针
    int cnt = 1;    // 入栈元素
    int num;    // 序列输入元素
    bool flag = 1;  // 序列实现可能性的标记
    for(int i=0; i<k; i++)
    {
        flag = 1;   // 重置标记
        top = 0;   // 清空堆栈后重新将 1 压入堆栈
        stack[top] = 1;
        cnt = 1;    // 入栈元素重置
        for(int j=0; j<n; j++)
        {
            cin >> num;
            if(stack[top] > num)    // 比栈顶元素小，必然不可能
            {
                flag = 0;
                continue;
            }
            while(stack[top] < num) // 比栈顶元素大
            {
                top++;  // 要继续压入数据
                cnt++;
                stack[top] = cnt;
            }
            if(top > m-1)   // 栈溢出
            {
                flag = 0;
                continue;
            }
            if(stack[top] == num)   // 等于栈顶元素，出栈
            {
                top--;
            }
            if(top < 0) // 栈内元素全部出栈
            {
                top++;
                cnt++;
                stack[top] = cnt;
            }
        }
        if(flag)
            cout << "YES" << endl;
        else
            cout << "NO" << endl;
    }

    return 0;
}
```