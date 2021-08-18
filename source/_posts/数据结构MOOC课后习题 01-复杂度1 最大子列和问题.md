---
title: 数据结构MOOC课后习题 01-复杂度1 最大子列和问题
date: 2021-08-12 23:12:57
updated: 2021-08-12 23:12:57
tags: 数据结构
categories: 数据结构
description:
---

# **01-复杂度1 最大子列和问题**

## 题目要求

给定*K*个整数组成的序列{ *N*<sub>1</sub>, *N*<sub>2</sub>, ..., *N*<sub>*K* </sub>}，“连续子列”被定义为{ *N*<sub>*i*</sub>, *N*<sub>*i*+1</sub>, ..., *N*<sub>*j* </sub>}，其中 1≤*i*≤*j*≤*K*。“最大子列和”则被定义为所有连续子列元素的和中最大者。例如给定序列{ -2, 11, -4, 13, -5, -2 }，其连续子列{ 11, -4, 13 }有最大的和20。现要求你编写程序，计算给定整数序列的最大子列和。

本题旨在测试各种不同的算法在各种数据情况下的表现。各组测试数据特点如下：

- 数据1：与样例等价，测试基本正确性；
- 数据2：10<sup>2</sup>个随机整数；
- 数据3：10<sup>3</sup>个随机整数；
- 数据4：10<sup>4</sup>个随机整数；
- 数据5：10<sup>5</sup>个随机整数；

### 输入格式:

输入第1行给出正整数*K* (≤100000)；第2行给出*K*个整数，其间以空格分隔。

### 输出格式:

在一行中输出最大子列和。如果序列中所有整数皆为负数，则输出0。

### 输入样例:

```in
6
-2 11 -4 13 -5 -2
结尾无空行
```

### 输出样例:

```out
20
结尾无空行
```

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int k;
    cin >> k;
    int maxsum = 0;
    int thissum = 0;
    int n;
    for(int i=1; i<=k; i++)
    {
        cin >> n;
        thissum += n;
        if(thissum < 0)
        {
            thissum = 0;
            continue;
        }
        else
        {
            if(thissum > maxsum)
            {
                maxsum = thissum;
            }
        }
    }
    cout << maxsum;
    
    return 0 ;
}
```

## 解题思路

使用“在线处理”方法，设置两个变量，maxsum（最大子列和），thissum（当前自列和）。循环输入数列数字，加入thissum（当前子列和），一旦thissum的大小小于0就说明之后再加任何的数字都只能变小，此时将thissum置0，进入下一次循环。每次循环更新maxsum大小，最后循环结束，输出maxsum。