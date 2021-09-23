---
title: 数据结构MOOC 03-树1 树的同构
date: 2021-08-22 17:36:54
updated: 2021-08-22 17:36:54
tags: 数据结构
categories: 数据结构
description:
---

# **03-树 1 树的同构**

## 题目要求

给定两棵树 T1 和 T2。如果 T1 可以通过若干次左右孩子互换就变成 T2，则我们称两棵树是“同构”的。例如图 1 给出的两棵树就是同构的，因为我们把其中一棵树的结点 A、B、G 的左右孩子互换后，就得到另外一棵树。而图 2 就不是同构的。

![fig1.jpg](https://img-blog.csdnimg.cn/img_convert/b064212154a4c309d741f770c240e271.png)

<center> 图 1</center>

![img](https://img-blog.csdnimg.cn/img_convert/d6062c798e72d4fa4e05212a61033c0b.png)

<center> 图 2</center>

现给定两棵树，请你判断它们是否是同构的。

### 输入格式:

输入给出 2 棵二叉树树的信息。对于每棵树，首先在一行中给出一个非负整数*N* (≤10)，即该树的结点数（此时假设结点从 0 到*N*−1 编号）；随后*N*行，第*i*行对应编号第*i*个结点，给出该结点中存储的 1 个英文大写字母、其左孩子结点的编号、右孩子结点的编号。如果孩子结点为空，则在相应位置上给出“-”。给出的数据间用一个空格分隔。注意：题目保证每个结点中存储的字母是不同的。

### 输出格式:

如果两棵树是同构的，输出“Yes”，否则输出“No”。

### 输入样例 1（对应图 1）：

```in
8
A 1 2
B 3 4
C 5 -
D - -
E 6 -
G 7 -
F - -
H - -
8
G - 4
B 7 6
F - -
A 5 1
H - -
C 0 -
D - -
E 2 -
```

### 输出样例 1:

```out
Yes
```

### 输入样例 2（对应图 2）：

```
8
B 5 7
F - -
A 0 3
C 6 -
H - -
D - -
G 4 -
E 1 -
8
D 6 -
B 5 -
E - -
H - -
C 0 2
G - 3
F - -
A 1 4
```

### 输出样例 2:

```out
No
```

### 解题思路：

题目要求判断按格式输入的两个二叉树是否同构，其实就是判断每个结点的左右子树是否相同（左右相同或者左左和右右分别相同），根据题目要求搭建程序框架：

```cpp
int main() {
	建二叉树 1
	建二叉树 2
	判断是否同构并输出
	
	return 0；
}
```

本题的程序框架较为简单，根据框架需要设计 `读数据建二叉树`、`二叉树同构判断`这两个函数。

① 读数据建二叉树的函数

根据题目的输入要求，我们可以使用静态链表即结构体数组的方式来存储二叉树。

```cpp
struct treeNode {
    char data;
    int left;
    int right;
}T1[10],T2[10];
```

输入样例中可以看到，二叉树根结点的位置不一定位于输入的第一行，因此在建二叉树的过程中，找出所建二叉树的根结点很关键。以简单的四个结点的二叉树为例：

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传 (img-7hGktDdw-1629624963590)(C:\Users\79485\AppData\Roaming\Typora\typora-user-images\image-20210820222827421.png)]

可以观察到，在结构体数组中四个结点分别占用了 0、1、2、3 四个下标位置，而结构体数组中左右子树指向的下标位置有 1、2、3，没有出现的 0 就是根结点所在的位置下标。因此，我们在建树的过程中寻找根结点的位置就可以通过遍历结构体数组中左右子树指向的下标，缺少的那个下标即是根结点所在下标。遍历的方法可以是创建一个 `check[]` 数组，初始化所有位置数值都为 0，然后在所有输入的左右子树的下标对应的 `check[]` 数值记为 1，最后遍历 `check[]` 数组找到数值为 0 的下标即是根结点所在位置。

```cpp
// 读取数据建二叉树的函数，返回根结点的下标
int buildTree(Tree t[])
{
    int N, root = -1;    // 结点数量，根结点位置下标
    cin >> N;
    char l, r;
    int check[10] = {0};
    for(int i=0; i<N; i++)
    {
        cin >> t[i].data >> l >> r;
        if (l != '-')
        {
            t[i].left = l - '0';    // 将字符转化为整型
            check[t[i].left] = 1;
        }
        else
            t[i].left = -1;
        if (r != '-')
        {
            t[i].right = r - '0';    // 将字符转化为整型
            check[t[i].right] = 1;
        }
        else
            t[i].right = -1;
    }
    for(int i=0; i<N; i++)
    {
        if(!check[i])
        {
            root = i;
            break;
        }
    }

    return root;
}
```

② 判断两二叉树是否同构的函数

判断二叉树是否同构就是判断两个二叉树的左右子树是否相同（可以是左左相同且右右相同，也可以是左右相同），使用递归判断就可以得出结论，但是要注意判断时要将各种情况考虑全面，包括子树为空时的各种情况。

```cpp
// 判断两二叉树是否同构的函数
int Isomorphic(int t1, int t2)
{
    if (t1 == -1 && t2 == -1)  // 均为空树
        return 1;
    if ((t1 == -1 && t2 != -1) || (t1 != -1 && t2 == -1))    // 两树中有一棵空树
        return 0;
    if (T1[t1].data != T2[t2].data)  // 根结点数据不同
        return 0;
    if (T1[t1].left == -1 && T2[t2].left == -1)    // 左子树均为空
        return Isomorphic(T1[t1].right,T2[t2].right);
    if ((T1[t1].left != -1 && T2[t2].left != -1) && (T1[T1[t1].left].data == T2[T2[t2].left].data) ) // 无需交换左右子树
        return Isomorphic(T1[t1].left,T2[t2].left) && Isomorphic(T1[t1].right,T2[t2].right);
    else    // 需要交换左右子树
        return Isomorphic(T1[t1].left,T2[t2].right) && Isomorphic(T1[t1].right,T2[t2].left);
}
```

### 代码：

```cpp
#include<bits/stdc++.h>
using namespace std;

typedef struct treeNode Tree;

struct treeNode {
    char data;
    int left;
    int right;
}T1[10],T2[10];

int buildTree(Tree t[]);  // 读取数据建二叉树
int Isomorphic(int t1, int t2);   // 判断两个二叉树是否同构的函数

int main()
{
    int t1, t2;
    t1 = buildTree(T1);
    t2 = buildTree(T2);
    if(Isomorphic(t1,t2))
        cout << "Yes";
    else
        cout << "No";

    return 0;
}

// 读取数据建二叉树的函数，返回根结点的下标
int buildTree(Tree t[])
{
    int N, root = -1;    // 结点数量，根结点位置下标
    cin >> N;
    char l, r;
    int check[10] = {0};
    for(int i=0; i<N; i++)
    {
        cin >> t[i].data >> l >> r;
        if (l != '-')
        {
            t[i].left = l - '0';    // 将字符转化为整型
            check[t[i].left] = 1;
        }
        else
            t[i].left = -1;
        if (r != '-')
        {
            t[i].right = r - '0';    // 将字符转化为整型
            check[t[i].right] = 1;
        }
        else
            t[i].right = -1;
    }
    for(int i=0; i<N; i++)
    {
        if(!check[i])
        {
            root = i;
            break;
        }
    }

    return root;
}

// 判断两二叉树是否同构的函数
int Isomorphic(int t1, int t2)
{
    if (t1 == -1 && t2 == -1)  // 均为空树
        return 1;
    if ((t1 == -1 && t2 != -1) || (t1 != -1 && t2 == -1))    // 两树中有一棵空树
        return 0;
    if (T1[t1].data != T2[t2].data)  // 根结点数据不同
        return 0;
    if (T1[t1].left == -1 && T2[t2].left == -1)    // 左子树均为空
        return Isomorphic(T1[t1].right,T2[t2].right);
    if ((T1[t1].left != -1 && T2[t2].left != -1) && (T1[T1[t1].left].data == T2[T2[t2].left].data) ) // 无需交换左右子树
        return Isomorphic(T1[t1].left,T2[t2].left) && Isomorphic(T1[t1].right,T2[t2].right);
    else    // 需要交换左右子树
        return Isomorphic(T1[t1].left,T2[t2].right) && Isomorphic(T1[t1].right,T2[t2].left);
}
```

