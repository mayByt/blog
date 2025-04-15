---
title: PTA团队天梯赛║L1-025 正整数A+B
date: 2021-08-05  19:31:27
updated: 2021-08-05  19:31:27
tags: PTA
categories: PTA刷题笔记
description:
---

# PTA团队天梯赛║L1-025 **正整数A+B** 

## 一、题目要求

题的目标很简单，就是求两个正整数`A`和`B`的和，其中`A`和`B`都在区间[1,1000]。稍微有点麻烦的是，输入并不保证是两个正整数。

### 输入格式：

输入在一行给出`A`和`B`，其间以空格分开。问题是`A`和`B`不一定是满足要求的正整数，有时候可能是超出范围的数字、负数、带小数点的实数、甚至是一堆乱码。

注意：我们把输入中出现的第1个空格认为是`A`和`B`的分隔。题目保证至少存在一个空格，并且`B`不是一个空字符串。

### 输出格式：

如果输入的确是两个正整数，则按格式`A + B = 和`输出。如果某个输入不合要求，则在相应位置输出`?`，显然此时和也是`?`。

### 输入样例1：

```in
123 456
```

### 输出样例1：

```out
123 + 456 = 579
```

### 输入样例2：

```
22. 18
```

### 输出样例2：

```
? + 18 = ?
```

### 输入样例3：

```
-100 blabla bla...33
```

### 输出样例3：

```
? + ? = ?
```

## 二、解题思路

思路 v1.0（未能完全通过测试点）

创建一个函数 *isint()* ，用于判断输入的是否为正整数，逐个字符判断是否为 `0~9` 之间的字符，一旦有一个字符不符合，返回值为0，否则返回值为1。在主函数中，将 *isint()* 的返回值赋给flag，在输出时，若flag为0则输出 `?` ，结果也输出 `?` 。

---

更新思路 v2.0，做完 L1-064 估值一亿的 AI 核心代码后，利用正则表达式重新完善该题。

题目有坑注意题目中`我们把输入中出现的第1个空格认为是`A`和`B`的分隔。题目保证至少存在一个空格，并且`B`不是一个空字符串。`的表述，A 串可为空串，并且B 串中可能出现多个空格的情况。因此需要找到输入的第一个空格将其分割为两个字符串。

利用 getline() 函数输入字符串，不能用 cin 会发生吞空格情况，遍历字符串，找到第一个空格所在下标，然后用substr() 函数切割成两个字符串检验。

检验其是否是[1,1000]的正整数，利用正则表达式将输入的字符串分四种情况分开讨论：

1. 一位数是可以是 1~9 ：`/^[1-9]/`
2. 两位数和三位数可以是 10~999：`/^[1-9][0-9]{1,2}/`
3. 最后是四位数的情况 1000：`/1000/`

利用 regex_match() 判断输入的格式是否正确，利用变量 flag 作为标志，若正确输出原值，不正确则输出 “?”。

## 三、代码

```cpp
//v1.0 得分11分 测试点1、3未通过
#include <iostream>
#include <cstring>
#include <stdlib.h>

using namespace std;

int isint(string s) //判断是否为整数
{
    bool k = 1;
    for(int i=0; i<s.length(); i++)
    {
        if(s[i]<'0' || s[i]>'9')
        {
            k = 0;
        }
    }
    if(k) return 1;
    else return 0;
}
int inrange(string s)   //判断是否在取值范围内
{
    bool k = 1;
    if(atoi(s.c_str()) < 1 || atoi(s.c_str()) > 1000)
    {
        k = 0;
    }
    if(k) return 1;
    else return 0;


}
int main()
{

    string a, b;
    cin >> a >> b;
    bool flag1 = isint(a);
    bool flag2 = isint(b);
    flag1 = inrange(a);
    flag2 = inrange(b);
    if(flag1) cout << a;
    else cout << "?";
    cout << " + ";
    if(flag2) cout << b;
    else cout << "?";
    cout << " = ";
    if(flag1 && flag2) cout << atoi(a.c_str())+atoi(b.c_str());
    else cout << "?";
    cout << endl;


    return 0;
}

```

```cpp
// v2.0 受 L1-064 的启发，使用正则表达式 AC 本题
#include <bits/stdc++.h>
using namespace std;

int main()
{
    regex pattern("(^[1-9]|[1-9][0-9]{1,2}|1000)");
    string s, a, b;
    getline(cin,s);
    int i;  // 第一个空格的下标
    for(i = 0; i<s.length(); i++)
    {
        if(s[i] == ' ')  // 找到字符串第一个空格的下标，分割两个字符串
            break;
    }
    a = s.substr(0,i);
    b = s.substr(i+1);
    bool flag1 = 0, flag2 = 0;
    if(regex_match(a,pattern))  flag1 = 1;
    if(regex_match(b,pattern))  flag2 = 1;
    if(flag1)
        cout << a;
    else
        cout << "?";
    cout << " + ";
    if(flag2)
        cout << b;
    else
        cout << "?";
    cout << " = ";
    if(flag1&flag2)
        cout << stoi(a)+stoi(b) << endl;
    else
        cout << "?" << endl;


    return 0;
}
```

## 四、反思总结

这道题目坑稍微有点多，测试点比较刁钻，注意审题的输入情况，对于字符串的输入空格控制比较严苛。利用 getline() 函数输入字符串，不能用 cin 会发生吞空格情况。使用正则表达式处理字符串匹配问题十分方便。

---

# 