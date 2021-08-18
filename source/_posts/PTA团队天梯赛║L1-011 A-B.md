---
title: PTA团队天梯赛║L1-011 A-B
date: 2021-01-12  17:34:09
updated: 2021-01-12  17:34:09
tags: PTA
categories: PTA刷题笔记
description:
---

# PTA团队天梯赛║L1-011 A-B

## 一、题目要求

本题要求你计算 *A*−*B* 。不过麻烦的是，*A*和*B*都是字符串 —— 即从字符串*A*中把字符串*B*所包含的字符全删掉，剩下的字符组成的就是字符串 *A*−*B* 。

### 输入格式：

输入在2行中先后给出字符串*A*和*B*。两字符串的长度都不超过104，并且保证每个字符串都是由可见的ASCII码和空白字符组成，最后以换行符结束。

### 输出格式：

在一行中打印出*A*−*B*的结果字符串。

### 输入样例：

```in
I love GPLT!  It's a fun game!
aeiou
```

### 输出样例：

```out
I lv GPLT!  It's  fn gm!
```

## 二、解题思路

设置一个flag，暴力循环比较a字符串的每一个字符是否与b字符串中的字符相同，如果发现相同，flag更新为false，最后用 **if(flag)** 控制输出a字符串

## 三、代码

```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main()
{
    string a, b;
    getline(cin,a);
    getline(cin,b);
    bool flag;
    size_t len = a.length();
    size_t len2 = b.length();
    for (int i = 0; i < len; ++i)
	{
        flag = true;
        for (int j = 0; j < len2; ++j)
		{
            if (b[j] == a[i])
			{
                flag = false;
                break;
            }
        }
        if (flag)
		{
            cout << a[i];
        }
    }
    cout << endl;

    return 0;
}

```

## 四、反思总结

程序时间复杂度为O(n<sup>2</sup>),程序耗时很长![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303191915276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDkyMTE4,size_16,color_FFFFFF,t_70)


网上看到柳诺大神巧妙的方法将时间复杂度降到O(n),思路与代码如下

> **分析：辣么多ASCII码也在0~255之间，所以用book数组标记所有的ASCII码～如果第二个字符出现了这个ACSII码那就标记为1~然后输出的时候当book数组对应的那个ASCII为1的时候就跳过不输出～**
>
> *引自https://www.liuchuo.net/archives/1597*

```cpp
#include <iostream>
using namespace std;
int book[256];
int main() {
    string s, a;
    getline(cin, s);
    getline(cin, a);
    for(int i = 0; i < a.length(); i++) book[a[i]] = 1;
    for(int i = 0; i < s.length(); i++) {
        if(book[s[i]] == 1) continue;
        cout << s[i];
    }
    return 0;
}
```

在测试点1和3耗时大为降低![](https://img-blog.csdnimg.cn/20210303191537645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDkyMTE4,size_16,color_FFFFFF,t_70)
