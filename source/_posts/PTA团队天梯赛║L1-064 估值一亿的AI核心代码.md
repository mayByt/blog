---
title: PTA团队天梯赛║L1-064 估值一亿的AI核心代码
date: 2021-08-05  16:57:17
updated: 2021-08-05  16:57:17
tags: PTA
categories: PTA刷题笔记
description:
---

# PTA团队天梯赛║L1-0**64 估值一亿的AI核心代码**

## 一、题目要求

本题要求你实现一个稍微更值钱一点的 AI 英文问答程序，规则是：

- 无论用户说什么，首先把对方说的话在一行中原样打印出来；
- 消除原文中多余空格：把相邻单词间的多个空格换成 1 个空格，把行首尾的空格全部删掉，把标点符号前面的空格删掉；
- 把原文中所有大写英文字母变成小写，除了 `I`；
- 把原文中所有独立的 `can you`、`could you` 对应地换成 `I can`、`I could`—— 这里“独立”是指被空格或标点符号分隔开的单词；
- 把原文中所有独立的 `I` 和 `me` 换成 `you`；
- 把原文中所有的问号 `?` 换成惊叹号 `!`；
- 在一行中输出替换后的句子作为 AI 的回答。

### 输入格式：

输入首先在第一行给出不超过 10 的正整数 N，随后 N 行，每行给出一句不超过 1000 个字符的、以回车结尾的用户的对话，对话为非空字符串，仅包括字母、数字、空格、可见的半角标点符号。

### 输出格式：

按题面要求输出，每个 AI 的回答前要加上 `AI:` 和一个空格。

### 输入样例：

```in
6
Hello ?
 Good to chat   with you
can   you speak Chinese?
Really?
Could you show me 5
What Is this prime? I,don 't know
```

### 输出样例：

```out
Hello ?
AI: hello!
 Good to chat   with you
AI: good to chat with you
can   you speak Chinese?
AI: I can speak chinese!
Really?
AI: really!
Could you show me 5
AI: I could show you 5
What Is this prime? I,don 't know
AI: what Is this prime! you,don't know
```

## 二、解题思路

本题需要大量的匹配字符串内容，由此可以想到利用正则表达式的匹配来实现匹配相应字符串内容并完成替换的功能，C++中提供了封装的 regex_replace() 函数，可以很好的完成该功能，接下来要做的就是如何合适地匹配以及替换来达到题目要求。

1. 首先消除字符串中的多余空格，利用 `/\s+/` 匹配多个空白字符，将其替换为单一空格 ' '。
2. 接着消除行首、行尾以及标点符号前的空格，`/^\s+|\s$|\s+(?=\W)/`，'^'表示行首，'$'表示行尾，'\W'表示非字母、数字、下划线的字符。
3. 将字符串中的所有 'I' 暂时替换为 "mark"，防止与后续转换 "can you" 时产生的 'I' 混淆，`/\bI\b/` ,‘\b' 是匹配单词的边界。
4. 接着将所有非 ’I‘ 的大写字母转换为小写字母。这里利用 string 的 tolower() 方法遍历字符串。
5. 然后匹配字符串中的 "can you" 和 "could you" ，`/\bcan you\b/` 和 `/\bcould you\b/` ,替换为 “ I can" 和 ”I could“。
6. 将原句中的 “I" 即现在的 “mark” 和 “me”，`/\bmark\b|\bme\b/`,转换为 “you” 。
7. 最后将 “?” 替换为 "!" 。

## 三、代码

```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int n;
    string s;
    cin >> n;
    getchar();
    while(n>0)
    {
        getline(cin,s);
        cout << s << endl;
        s = regex_replace(s, regex(R"(\s+)"), " ");
        s = regex_replace(s, regex(R"(^\s+|\s$|\s+(?=\W))"), "");
        s = regex_replace(s, regex(R"(\bI\b)"), "mark");
        for(int i=0; i<s.length();i++)
        {
            if(s[i] != 'I')
                s[i] = tolower(s[i]);
        }
        s = regex_replace(s, regex(R"(\bcan you\b)"), "I can");
        s = regex_replace(s, regex(R"(\bcould you\b)"), "I could");
        s = regex_replace(s, regex(R"(\bmark\b|\bme\b)"), "you");
        s = regex_replace(s, regex(R"(\?)"), "!");
        cout << "AI: " << s << endl;
        n--;
    }

    return 0;
}
```

## 四、反思总结

本题使用到了正则表达式，要对于正则表达式的语法熟悉，并且会调用 C++ 中的 regex 相关方法，一开始踩了坑，在调用 regex_replace()函数时对于其参数的正确格式没有掌握，导致一直报错，以下为 [C++ 中 regex 方法的使用的相关资料](https://blog.csdn.net/qq_34802416/article/details/79307102?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_default&utm_relevant_index=2)、[正则表达式基本语法](https://www.runoob.com/regexp/regexp-metachar.html)。

同时，通过做本题，对于之前卡壳无法 AC 的 L1-025 **正整数A+B** ，也尝试使用正则表达式的方法解题。