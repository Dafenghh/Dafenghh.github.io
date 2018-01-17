---
title: 'POJ 3368 [Frequent Values] 题解'
comments: true
categories: ACM
tags:
  - data structures
  - segment tree
abbrlink: c9635eac
date: 2018-01-11 09:54:21
---
# 题目大意
给定一个长度为N的单调不下降的数组，M次询问，每次询问原数组区间[i, j]中，出现频数最多的数的频数。

# 题目分析
先使用离散化，然后用lower_bound很方便的求出每一个值在原数组中的始点和终点。（终点可以用下一个值的始点表示）

然后构造一棵值分布的线段树，也就是说这棵线段树的每一个结点维护的是它所对应区间[l, r）中的值的最大的出现频数。

询问区间[i, j]， 先求出a[i], a[j]在[i,j]中的出现频数（用第一步求出的a[i]、a[j]的始点、终点位置很容易得到结果）。

然后往线段树中查询[a[i] + 1, a[j])即可。

# source code
```c++
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <iostream>
using namespace std;
int N, n, Q;
inline int read()
{
    int X=0,w=0; char ch=0;
    while(!isdigit(ch)) {w|=ch=='-';ch=getchar();}
    while(isdigit(ch)) X=(X<<3)+(X<<1)+(ch^48),ch=getchar();
    return w?-X:X;
}
const int maxn = 100010;
int a[maxn], b[maxn], lt[maxn], tr[maxn * 2];//left

void build(int v = 1, int l = 0, int r = n) { // [l, r)
    if (l + 1 == r) {
        tr[v] = lt[r] - lt[l];
    }
    else {
        int mid = (l + r) >> 1, chl = v * 2, chr = chl + 1;
        build(chl, l, mid);
        build(chr, mid, r);
        tr[v] = max(tr[chl], tr[chr]);
    }
}

int query(int L, int R, int v = 1, int l = 0, int r = n) {// query [L, R) 
    if (l >= r || R <= l || r <= L) return -12344;
    if (L <= l && r <= R) return tr[v];
    int mid = (l + r) >> 1, chl = v * 2, chr = chl + 1;
    return max(query(L, R, chl, l, mid), query(L, R, chr, mid, r));
}

void update(int &a, int b) {
    if (b > a) a = b;
}

int main() {
    for(;;) {
        N = read();
        if (!N) break;
        Q = read();
        for (int i = 0; i < N; i++) a[i] = b[i] = read();
        n = unique(b, b + N) - b;
        for (int i = 0; i < N; i++) a[i] = lower_bound(b, b + n, a[i]) - b;
        for (int i = 0; i < n; i++) lt[i] = lower_bound(a, a + N, i) - a;
        lt[n] = N;
        build();
        while (Q--) {
            int l = read(), r = read(); //[l, r]
            l--;
            r--;
            int ans = -1244;
            if (a[l] == a[r]) ans = r - l + 1;
            else {
                update(ans, lt[a[l] + 1] - l);
                update(ans, r - lt[a[r]] + 1);
                int L = a[l] + 1, R = a[r]; // [L, R)
                if (L < R)
                update(ans, query(L, R));
            }
            printf("%d\n", ans);
        }
    }
}
```