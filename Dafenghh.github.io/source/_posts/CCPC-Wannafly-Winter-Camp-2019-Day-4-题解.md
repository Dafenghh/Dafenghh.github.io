---
title: CCPC-Wannafly Winter Camp 2019 Day 4 题解
comments: true
categories: ACM
tags:
  - DP
abbrlink: 5505493b
date: 2019-01-25 07:49:58
---


# H 命命命运

## 题意
wls等六人在玩大富翁。

大富翁的棋盘上一共有n块地，这些地围成了一个圈（n号地的下一块地是1号地）。

每一轮，六个玩家会依次掷出一颗骰子（编号小的玩家先掷），并往前走骰子上显示的数字那么多步。

第一个踩到某块地的玩家能够得到这块地。

给出`p[i][j]`, 表示第i个玩家掷出j的概率。

现在请问，500轮以后，每个玩家拥有的地的块数的期望分别是多少？

所有人都从1号点出发，出发前大家不能买地（第二次到1号点才能买这块地）。

## 分析
我们需要求出对于i号玩家，第j轮，获得第k块地的概率，然后把所有j,k取值下的概率累加起来，就是每个人获得地的块数的期望。i号玩家，获得第j块地等价于`1..(i-1)`号玩家前j轮不曾走到k，i号玩家第j轮第一次走到k，`(i+1)..6`号玩家前(j-1)轮未曾走到k.

于是我们要求出两个概率来，`never[i][j][k]`表示i号玩家前j轮未曾走到k的概率，`first[i][j][k]`表示i号玩家第j轮第一次走到k的概率。

为了求出这两个东西，我们还需要一维状态，即玩家当前所处的位置。所以`pos[i][j][k][m]`表示i号玩家前j轮未曾走到k，走完第i轮后位于m的概率。

转移式很好写出来。于是我们得到了一个个复杂度`O(6*6*500*n^2)`的优秀算法, n为500， 爆了。

那么怎么优化这个东西呢？

突破口在于，pos数组的k只需要求前7块地就可以了。这样由pos的结果得到前7块地的first和never值，而后面8..n块地的first,never值可以直接由前面7块地的first,never值分别递推出来。因为每一轮掷骰子的概率是独立的，跟所处位置无关，j轮移动可以看成1+(j-1)轮，即走完第一轮再走j-1轮。例如，我们求`first[i][j][k]`, 可以枚举第一轮移动的步数t，贡献就是`p[i][t]*first[i][j-1][k-t]`. never同理。

于是我们得到了正解。

## 源代码
``` c++
#include <bits/stdc++.h>
using namespace std;
const int N = 502;

int n;
double p[6][6], first[6][N][N], never[6][N][N], pos[6][N][7][N];

void solve(double p[], double first[N][N], double never[N][N], double pos[N][7][N]) {
    for (int j = 0; j <= 6; j++) pos[0][j][0] = 1;

    for (int i = 0; i <= 500; i++)
    for (int j = 0; j <= 6; j++)
    for (int k = 0; k < n; k++) {
        never[i][j] += pos[i][j][k];
        for (int t = 0; t < 6; t++) {
            int nk = (k+t+1)%n;
            if (nk == j) first[i+1][j] += pos[i][j][k] * p[t];
            else pos[i+1][j][nk] += pos[i][j][k] * p[t];
        }
    }

    for (int k = 0; k < n; k++) never[0][k] = 1;

    for (int i = 1; i <= 500; i++)
    for (int j = 7; j < n; j++) 
    for (int t = 0; t < 6; t++) {
        first[i][j] += p[t] * first[i-1][j-t-1];
        never[i][j] += p[t] * never[i-1][j-t-1];
    }
}

double ans[6];

int main() {
    scanf("%d", &n);
    for (int i = 0; i < 6; i++)
    for (int j = 0; j < 6; j++)
        scanf("%lf", &p[i][j]);

    for (int i = 0; i < 6; i++) solve(p[i], first[i], never[i], pos[i]);

    for (int i = 0; i < 6; i++)
    for (int j = 1; j <= 500; j++)
    for (int k = 0; k < n; k++) {
        double prob = first[i][j][k];
        for (int t = 0; t < i; t++) prob *= never[t][j][k];
        for (int t = i+1; t < 6; t++) prob *= never[t][j-1][k];
        ans[i] += prob;
    }
    for (int i = 0; i < 6; i++) printf("%.3lf\n", ans[i]); 
}
```