---
title: 'POJ 3264 [Balance Lineup] 题解'
comments: true
date: 2017-12-29 20:56:34
categories: ACM
tags:
- segmentTree
---
# 题目大意
长度为N的数组，Q次查询，查询区间[A, B]的最大值与最小值之差。

<!-- more -->

# 题目分析
线段树模板题。


# source code
```c++
#include <cstdio>
#include <algorithm>
using namespace std;
const int maxn = 100050;
const int INF = 1234567;
int mx[2 * maxn], lx[2 * maxn];
int n;
void update(int x, int y, int v = 0, int l = 0, int r = n) { // a[x] = y;
   // printf("v = %d, l = %d, r = %d\n", v, l, r);
    if (l >= r || x < l || x >= r) return;
    int chl = v * 2 + 1, chr = v * 2 + 2, mid = (l + r) / 2;
    if (l + 1 == r) {
        mx[v] = lx[v] = y;
    }
    else {
        update(x, y, chl, l, mid);
        update(x, y, chr, mid, r);
        mx[v] = max(mx[chl], mx[chr]);
        lx[v] = min(lx[chl], lx[chr]);
    }
}

int queryMax(int L, int R, int v = 0, int l = 0, int r = n) {
    if (l >= r || R <= l || L >= r) return -INF;
    if (L <= l && r <= R) return mx[v];
    int chl = v * 2 + 1, chr = v * 2 + 2, mid = (l + r) / 2;
    return max(queryMax(L, R, chl, l, mid), queryMax(L, R, chr, mid, r));
}

int queryMin(int L, int R, int v = 0, int l = 0, int r = n) {
    if (l >= r || R <= l || L >= r) return INF;
    if (L <= l && r <= R) return lx[v];
    int chl = v * 2 + 1, chr = v * 2 + 2, mid = (l + r) / 2;
    return min(queryMin(L, R, chl, l, mid), queryMin(L, R, chr, mid, r));
}


int main() {
    int q;
    scanf("%d%d", &n, &q);
    fill(mx, mx + 2 * maxn, -INF);
    fill(lx, lx + 2 * maxn, INF);
    int y;
    for (int i = 0; i < n; i++) {
        scanf("%d", &y);
        update(i, y);
    }
    while (q--) {
        int A, B;
        scanf("%d%d", &A, &B);
        int L = A - 1, R = B;
        printf("%d\n", queryMax(L, R) - queryMin(L, R));
    }
}
```