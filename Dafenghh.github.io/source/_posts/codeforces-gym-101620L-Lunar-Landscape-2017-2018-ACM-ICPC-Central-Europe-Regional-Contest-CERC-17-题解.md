---
title: >-
  codeforces gym 101620L [Lunar Landscape] 2017-2018 ACM-ICPC, Central Europe
  Regional Contest (CERC 17) 题解
comments: true
categories: ACM
tags:
  - math
abbrlink: '96671495'
date: 2018-01-26 13:40:19
---

# 题目大意
卫星拍下了地面的很多照片。每张照片覆盖了平面的一个正方形区域，这个正方形要么边与坐标轴平行，要么对角线与坐标轴平行，保证中心点和顶点都在坐标整点的位置。

求被覆盖的区域面积。


<!-- more -->


# 题目分析

如果题目只给出边与坐标轴平行的正方形的话，那这道题很简单，直接二维前缀和就OK。

但这道题有侧着放的正方形（对角线与坐标轴平行），又该怎么处理呢？

首先发现，一个正方形小格可以按对角线分成4块小三角形，有多少块小三角形被覆盖，则要看侧正方形的分布情况。

如果用一下坐标变换，将坐标系向由旋转45度，再将单位长度设置为小三角形的直角边长。可以求出，这个坐标变换的表达式为：
$x'=x-y,y'=x+y$

旋转坐标系后，将每一个小三角形视为一个元素，然后对这些小三角形用二维前缀和即可。

找出变换后的坐标跟原来坐标的对应关系，本题解答完成。

# source code
```c++
#include <cstdio>
#include <iostream>
using namespace std;
const int N = 1600;
int f[2 * N][2 * N], g[8 * N][4 * N];

int main() {
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++){
        char s[10];
        int x,y,d;
        scanf("%s%d%d%d",s, &x,&y,&d);
        d /= 2;
        if (s[0] == 'A') {
            f[x-d+N][y-d+N]++;
            f[x+d+N][y-d+N]--;
            f[x-d+N][y+d+N]--;
            f[x+d+N][y+d+N]++;
        }
        else {
            int x0 = x - y, y0 = x + y;
            x = x0, y = y0;
            g[2*x-2*d+4*N][y-d+2*N]++;
            g[2*x+2*d+4*N][y-d+2*N]--;
            g[2*x-2*d+4*N][y+d+2*N]--;
            g[2*x+2*d+4*N][y+d+2*N]++;
        }
    }
    for (int i = 0; i < 2 * N; i++)
    for (int j = 0; j < 2 * N; j++) {
        if (i > 0) f[i][j] += f[i-1][j];
        if (j > 0) f[i][j] += f[i][j-1];
        if (i > 0 && j > 0) f[i][j] -= f[i-1][j-1];
    }

    for (int i = 0; i < 8 * N; i++)
    for (int j = 0; j < 4 * N; j++) {
        if (i > 0) g[i][j] += g[i-1][j];
        if (j > 0) g[i][j] += g[i][j-1];
        if (i > 0 && j > 0) g[i][j] -= g[i-1][j-1];
    }
    double ans = 0;
  
    for (int x = -1510; x < 1510; x++)
    for (int y = -1510; y < 1510; y++) {
        if (f[x+N][y+N]) ans++;
        else {
            int x0 = x - y, y0 = x + y;
            if (g[2*x0-1+4*N][y0+1+2*N]) ans += 0.25;
            if (g[2*x0-1+4*N][y0+2*N]) ans += 0.25;
            if (g[2*x0+4*N][y0+1+2*N]) ans += 0.25;
            if (g[2*x0+4*N][y0+2*N])   ans += 0.25;
        }
    }
    printf("%.2lf\n", ans);
    
}

```
