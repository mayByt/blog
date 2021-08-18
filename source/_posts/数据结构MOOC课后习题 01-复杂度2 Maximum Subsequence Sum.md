---
title: 数据结构MOOC课后习题 01-复杂度2 Maximum Subsequence Sum
date: 2021-08-12 23:14:08
updated: 2021-08-12 23:14:08
tags: 数据结构
categories: 数据结构
description:
---

# **01-复杂度2 Maximum Subsequence Sum**

## 题目要求

Given a sequence of *K* integers { *N*<sub>1</sub>, *N*<sub>2</sub>, ..., *N*<sub>*K* </sub>}. A continuous subsequence is defined to be { *N*<sub>*i*</sub>, *N*<sub>*i*+1</sub>, ..., *N*<sub>*j* </sub> } where 1≤*i*≤*j*≤*K*. The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements. For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.

### Input Specification:

Each input file contains one test case. Each case occupies two lines. The first line contains a positive integer *K* (≤10000). The second line contains *K* numbers, separated by a space.

### Output Specification:

For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line. In case that the maximum subsequence is not unique, output the one with the smallest indices *i* and *j* (as shown by the sample case). If all the *K* numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

### Sample Input:

```in
10
-10 1 2 3 4 -5 -23 3 7 -21
```

### Sample Output:

```out
10 1 4
```

### Code：

```cpp
#include<bits/stdc++.h>
using namespace std;

int a[10005];

int main(){
    int n;
    cin >> n;
    for(int i=1;i<=n;i++) cin>>a[i];
    int MaxSum = -1;
    int ThisSum = 0;
    int left = 1, right = n, templeft = 1, tempright = 1;
    for(int i=1;i<=n;i++){
    	tempright = i;
    	ThisSum += a[i];
    	if(ThisSum > MaxSum){
    		left = templeft;
            right = tempright;
    		MaxSum = ThisSum;
		}
    	if(ThisSum<0){
    		ThisSum = 0;
    		templeft = i+1;
    		tempright=i+1;
		} 
	}
	if(MaxSum<0) cout << 0 << " " << a[1] << " " << a[n] << endl;
    else cout << MaxSum << " " << a[left] << " " <<a [right] << endl;
    
	return 0;
}
```

### 解题思路：

这道题是上一道题的难度加大版，需要记录最大子列和首尾的数字，因此需要构建一个数组来存放数字序列以方便计数，注意这里有两个坑，测试时多次未AC。

① 如果序列全为负数时，要输出 ”0 序列首位数字 序列末尾数字“，如果序列中有0存在，其他数字均为负数时，则输出” 0 0 0 “。

② 如果存在多个并列的最大子列和，要输出下标更小的那一组。

---