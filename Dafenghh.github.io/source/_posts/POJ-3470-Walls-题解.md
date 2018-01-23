---
title: 'POJ 3470 [Walls] 题解'
comments: true
categories: ACM
tags:
  - segment tree
  - scanline
abbrlink: 7d2ff651
date: 2018-01-22 23:11:51
---
# 题目大意
平面上有n条与坐标轴平行的互不相交的线段，代表墙；还有m个不在线段上的点，代表鸟。鸟会沿着坐标轴方向，飞向离它最近的墙，并撞上去。求每一面墙会有多少只鸟撞上来。

# 题目分析
先离散化坐标。

按其中一个坐标（比如y）排好序后，使用扫描线算法，对于另一个坐标（比如x）建一棵支持区间更新和单点查询的线段树，表示每一个点当前会撞向的墙的序号。

自己写的扫描线啰嗦地惨不忍睹，WA很多次都没过。

参考了下[forever97](http://www.cnblogs.com/forever97/p/poj3470.html) 的代码，非常简洁优雅，落落大方。真是感激不尽！


# source code
```c++
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
using namespace std;
typedef long long ll;
const int maxn = 200020;
int tr[maxn * 2];

int query(int x, int v = 0, int l = 0, int r = maxn - 20) {
    if(tr[v] != -2) return tr[v];
    if (l + 1 == r) return tr[v];
    int mid = (l + r) >> 1, chl = v * 2 + 1, chr = v * 2 + 2;
    if (x < mid) return query(x, chl, l, mid);
    return query(x,chr,mid, r);
}

void update(int L, int R, int val, int v = 0, int l = 0, int r = maxn - 20) {
    if (r <= L || R <= l) return;
    if (L <= l && r <= R) {
        tr[v] = val;
        return;
    }
    int mid = (l + r) >> 1, chl = v * 2 + 1, chr = v * 2 + 2;
    if (tr[v] != -2) {
        tr[chl] = tr[chr] = tr[v];
        tr[v] = -2;
    }
    update(L, R, val, chl, l, mid);
    update(L, R, val, chr, mid, r);   
}

int X[maxn], Y[maxn], X_inx[maxn], Y_inx[maxn],X_comp[maxn], Y_comp[maxn],
    wall_to_fly[maxn], total_for_wall[maxn],dis[maxn];
int *arr;
int wn,pn; // wall point
bool cmp(int a, int b) {
    return arr[a] < arr[b];
}
void compress(int *X, int *Comp,int *Inx, int n=pn) {
    arr = X;
    for (int i = 0; i < n; i++) Inx[i] = i;
    sort(Inx, Inx + n, cmp);
    int cnt = 0;Comp[Inx[0]] = 0;
    for (int i = 1; i < n; i++) {
        if (X[Inx[i]] != X[Inx[i-1]]) cnt++;
        Comp[Inx[i]] = cnt;
    }
}

void scan(int *X, int *Y, int *X_comp, int i) {
    if (i < wn) {
        int i_ = i^1;
        if (X_comp[i_] >= X_comp[i]) {
            update(X_comp[i], X_comp[i_]+1,i/2);
        }
    }else {
        int q = query(X_comp[i]);
        if (q != -1) {
            int d = min(abs(Y[i] - Y[2*q]), abs(Y[i] - Y[2*q+1]));
            i-=wn;
            if (dis[i] == -1 || d < dis[i]) {
                dis[i] = d;
                wall_to_fly[i] = q;
            }
        }
    }
}

void fly(int *X, int *Y, int *X_comp, int *Inx) {
    tr[0] = -1;
    for (int i = 0; i < pn; i++) scan(X, Y, X_comp, Inx[i]);
    tr[0] = -1;
    for (int i = pn; i >= 0; i--) scan(X, Y, X_comp, Inx[i]);
}


int main() {
    memset(dis, -1, sizeof(dis));
    int n_, m_;
    scanf("%d%d", &n_, &m_);
    wn = 2 * n_;
    pn = wn + m_;
    for (int i = 0; i < pn; i++)
        scanf("%d%d", X + i, Y + i);
    compress(X, X_comp, X_inx);
    compress(Y, Y_comp, Y_inx);
    fly(X, Y, X_comp, Y_inx);
    fly(Y, X, Y_comp, X_inx);
    for (int i = 0; i < m_; i++) total_for_wall[wall_to_fly[i]]++;
    for (int i = 0; i < n_; i++) printf("%d\n", total_for_wall[i]);
}
```