---
title: >-
  codeforces gym 101612 K [Kotlin Island] (ICPC 2017-2018 NEERC Northern
  Subregional Contest St Petersburg November 4 2017) 题解
comments: true
date: 2017-11-29 22:36:06
categories: ACM
tags:
- brute force
---

## 题目描述
gym 101612 K题
链接： [gym](http://codeforces.com/gym/101612)

一个岛可以看成一个h*w的网格，现在可以在任意的行或者任意的类挖水渠，目标是将网格剩下没被挖水渠的点划分成k个连通块。给出一种方案即可。

<!-- more -->

## 题目分析

显然最后分成的连通块数目 = 行的分块数 * 列的分块数。比如有5行，那么我们最多可以分成3块。

枚举一遍就出答案了。

## source code
```c++
#include <cstdio>
const int maxn = 120;
bool row[maxn], col[maxn];
int h, w, n;
int main() {
    #ifndef LOCAL
        freopen("kotlin.in", "r", stdin);
        freopen("kotlin.out", "w", stdout);
    #endif
    scanf("%d%d%d", &h, &w, &n);
    for (int hi = 1; hi <= (h + 1) / 2; hi++)
    for (int wi = 1; wi <= (w + 1) / 2; wi++) {
        if (hi * wi == n){
            for (int i = 0; i < hi - 1; i++) row[1 + i * 2] = true;
            for (int i = 0; i < wi - 1; i++) col[1 + i * 2] = true;
            
            for (int i = 0; i < h; i++) {
                for (int j = 0; j < w; j++){
                    if (row[i] || col[j]) printf("#");
                    else printf(".");
                }
                printf("\n");
            }
            return 0;
        }
    }
    printf("Impossible\n");
}


```