---
title: PTA团队天梯赛║L1-032 Left-pad
date: 2021-01-20  15:31:34
updated: 2021-01-20  15:31:34
tags: PTA
categories: PTA刷题笔记
description:
---

# PTA 团队天梯赛║L1-032 **Left-pad**

## 一、题目要求

根据新浪微博上的消息，有一位开发者不满 NPM（Node Package Manager）的做法，收回了自己的开源代码，其中包括一个叫 left-pad 的模块，就是这个模块把 javascript 里面的 React/Babel 干瘫痪了。这是个什么样的模块？就是在字符串前填充一些东西到一定的长度。例如用`*`去填充字符串`GPLT`，使之长度为 10，调用 left-pad 的结果就应该是`******GPLT`。Node 社区曾经对 left-pad 紧急发布了一个替代，被严重吐槽。下面就请你来实现一下这个模块。

### 输入格式：

输入在第一行给出一个正整数`N`（≤104）和一个字符，分别是填充结果字符串的长度和用于填充的字符，中间以 1 个空格分开。第二行给出原始的非空字符串，以回车结束。

### 输出格式：

在一行中输出结果字符串。

### 输入样例 1：

```in
15 _
I love GPLT
```

### 输出样例 1：

```out
____I love GPLT
```

### 输入样例 2：

```
4 *
this is a sample for cut
```

### 输出样例 2：

```out
 cut
```

## 二、解题思路

先比较输入字符串长度与输出字符数的大小，若字符串长度大于输出字符，则从 `字符串长度-输出字符数`处开始输出直至字符串结束，若字符串长度小于输出字符则先输出 `输出字符数-字符串长度` 个符号再输出字符串。

## 三、代码

```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
    string s;
    int num, dif;
    char c;
    cin >> num >> c;
    getchar();
    getline(cin,s);
    if(s.length()>num)
    {
        dif = s.length()-num;
        for(int i=dif; i<s.length(); i++)
        {
            cout << s[i];
        }
    }
    else
    {
        dif = num-s.length();
        for(int i=0; i<dif; i++)
        {
            cout << c;
        }
        cout << s;
    }
    cout << endl;

    return 0;
}

```

## 四、反思总结

在输入字符串时 `cin` 输入流遇到空字符会结束，现用 getchar() 清除缓冲区，利用 getline() 写入直到遇到换行符结束。