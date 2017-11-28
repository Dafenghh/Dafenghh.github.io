---
title: 'codeforces 894C [Marco and GCD Sequence] #round 447 div2C 题解'
comments: true
categories: ACM
tags:
  - constructive algorithms
  - math
abbrlink: d93115b9
date: 2017-11-25 22:04:20
---

## 题目分析
将输入的数组求一次gcd, 设求出来的值为s，然后判断s是否在原来的数组中。如果不在，那么无解；否则可以构造出一组解，往输入数组的每两个相邻的数中插入一个s，然后就得到一组解。

<!-- more -->

简单证明这组解是正确的：

设输入数组为a[]，依照上面的方式构造出来的数组为b[]，下面证明：b数组中任意一个区间的gcd值恰好构成集合a。

若i==j，即区间长度为1，那么gcd(b[i]) = b[i]， 而b[i]构成的集合与集合a等价，即表明a[]中的值都是b数组一个区间的gcd值；
若i!=j，那么gcd(b[i..j]) = s, s属于集合a。
以上两点说明b[]就是答案。

## source code
```c++
#include <cstdio>
#include <set>
using namespace std;

int gcd(int a, int b){
    return b == 0?a:gcd(b, a%b);
}
int main()
{
    int m;
    int a[2000];
    scanf("%d", &m);
    set<int> S;
    for (int i = 0; i < m; i++){
        scanf("%d", a + i);
        S.insert(a[i]);
    } 
    int g = a[0];
    for (int i = 1; i < m; i++) g = gcd(g, a[i]);
    if (S.count(g)){
        printf("%d\n%d", 2 * m - 1, a[0]);
        for (int i= 1; i < m; i++) printf(" %d %d", g, a[i]);
        printf("\n");
    }
    else printf("-1\n");
}
```

