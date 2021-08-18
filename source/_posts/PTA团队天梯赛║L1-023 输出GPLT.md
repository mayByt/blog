---
title: PTA团队天梯赛║L1-023 输出GPLT
date: 2021-01-16  20:21:34
updated: 2021-01-16  20:21:34
tags: PTA
categories: PTA刷题笔记
description:
---

# PTA团队天梯赛║L1-023  **输出GPLT**

## 一、题目要求

给定一个长度不超过10000的、仅由英文字母构成的字符串。请将字符重新调整顺序，按`GPLTGPLT....`这样的顺序输出，并忽略其它字符。当然，四种字符（不区分大小写）的个数不一定是一样多的，若某种字符已经输出完，则余下的字符仍按`GPLT`的顺序打印，直到所有字符都被输出。

### 输入格式：

输入在一行中给出一个长度不超过10000的、仅由英文字母构成的非空字符串。

### 输出格式：

在一行中按题目要求输出排序后的字符串。题目保证输出非空。

### 输入样例：

```in
pcTclnGloRgLrtLhgljkLhGFauPewSKgt
```

### 输出样例：

```out
GPLTGPLTGLTGLGLL
```

## 二、解题思路

先将输入的字符串全部转换为大写字母，然后对于GPLT四个字母计数，将每个字母出现的次数存放在a[4]中，然后按照顺序输出 `GPLT` ，每输出一个字母，该字母对应的计数减一，直到减为0。

## 三、代码

```cpp
#include <iostream>
#include <cctype>
#include <cstring>
using namespace std;

int main()
{
    int a[4] = {0};   //存放GPLT出现次数
    string s;
    cin >> s;
    for(int i=0; i<s.length(); i++) //将字符串全部转化为大写字母，并计数
    {
        s[i] = toupper(s[i]);
        if(s[i] == 'G') a[0]++;
        else if(s[i] == 'P') a[1]++;
        else if(s[i] == 'L') a[2]++;
        else if(s[i] == 'T') a[3]++;
    }

    //输出
    while(a[0]!=0 || a[1]!=0 || a[2]!=0 || a[3]!=0)
    {
        for(int i=0; i<4; i++)
        {
            if(a[i] != 0)
        {
            a[i]--;
            if(i == 0) cout << "G";
            else if(i == 1) cout << "P";
            else if(i == 2) cout << "L";
            else if(i == 3) cout << "T";
        }
        }
    }
    cout << endl;

    return 0;
}

```

## 四、反思总结

利用了转换大写字母的函数 *toupper()* ，包含在头文件 `<cctype>` 中。


