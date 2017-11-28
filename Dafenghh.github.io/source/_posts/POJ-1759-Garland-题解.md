---
title: 'POJ 1759 [Garland] 题解'
comments: true
categories: ACM
tags:
  - math
  - binary search
abbrlink: 68c8caf8
date: 2017-11-28 10:08:39
---
## 题目大意
给定一个数列H[]，满足下面的关系：
```
H[1] = A
H[i] = (H[i-1] + H[i+1])/2 - 1, for all 1 < i < N 
H[N] = B 
H[i] >= 0, for all 1 <= i <= N 
```
已知A，求B的最小值。

<!-- more -->

## 题目分析
化成递推式， 有` H[i+1] = 2 * H[i] - H[i-1] + 2 `

作下变形，有` H[i+1] - H[i] = H[i] - H[i-1] + 2 ` 

所以H[i+1] - H[i]是一个等差数列，公差为2，那么：` H[i+1] - H[i] = H[2] - H[1] + 2 * (i - 1) ` 

做累加求和即得H[i]通项公式： ` H[i] = (i - 1) * H[2] + (2 - i) * H[1] + (i - 1) * (i - 2) ` 

题目要求B，即` H[N] = (N - 1) * H[2] + (2 - N) * H[1] + (N - 1) * (N - 2) ` 

这个式子中，仅有H[2]为未知项，其他均已知。而且H[2]项前的系数N-1为正，H[N]与H[2]线性正相关，所以H[N]取最小值当且仅当H[2]取最小值。

于是我们将问题转化为求H[2]最小值即可。对H[2]进行二分，每次验证数列里的所有项是否全部大于零即可。

P.S. 交了几发WA，因为一不小心交了G++编译器，POJ的G++编译器有点老，浮点支持得不好。


## source_code
```c++
#include <cstdio>
#include <cmath>
int n;
double A,B;
bool judge(double mid) {
    double a = A, b = mid,x;
    for (int i = 3; i <= n; i++) {
        x = 2 * b - a + 2;
        if (x < 0) return false;
        a = b;
        b = x;
    }
    B = x;
    return true;
}

int main() {
    scanf("%d%lf", &n, &A);
    double L = -1, R = 1060;
    for (int i = 0; i < 100; i++) {
        double mid = (L + R) / 2;
        if (judge(mid)) R = mid;
        else L = mid;
    }
    printf("%.2lf\n", B);
}
```
