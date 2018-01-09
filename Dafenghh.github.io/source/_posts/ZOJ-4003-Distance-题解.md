---
title: 'ZOJ 4003 [Distance] 题解'
comments: true
categories: ACM
tags:
  - 尺取法
abbrlink: 6c754de8
date: 2018-01-09 13:05:12
---
# 题目大意
定义两个维度为n的向量 $(a_1,a_2,a_3\dots a_n)$ 和 $(b_1,b_2,b_3\dots b_n)$ 间的距离为 $\sum _{i=1} ^n \left | a_i - b_i \right |^p$

现在给出向量$X = (x_1, x_2, x_3 \dots x_n)$ 和 $Y = (y_1, y_2, y_3 \dots y_n)$

定义子向量（subvector）为原向量的项中连续的一段。从X中取出一个子向量x, 从Y中取出一个子向量y，使得x和y长度相同，x和y的
距离小于给定值V。求这样的子向量对(x, y)的个数。

<!-- more -->

# 题目分析
作一个n*n的矩阵`diff[][]`，记录X中的每一项到Y中的每一项的距离，

即 `diff[i][j] = |X[i] - Y[j]| ^ p `

取出这个矩阵的每一条斜线的值，用尺取法统计结果即可。

P.S. 写这题时犯了很多智障错误。如%d写成%p，return res写成return x。

诸如此类。

如何避免这类typo的发生呢？

这是一个问题。

# source code
```c++
#include <cstdio>
#include <algorithm>
using namespace std;
typedef long long ll;

ll Pow(ll x, ll y) {
    ll res = 1;
    while (y--) res *= x;
    return res;
}

int count(ll a[], int n, ll V) {
    ll sum = 0;
    int r = 0, ans = 0;
    for (int l = 0; l < n; l++) {
        r = max(r, l);

        while (r < n && sum + a[r] <= V) {
            sum += a[r];
            r++;
        }
        ans += r - l;
        if (r > l) sum -= a[l];
    }
    return ans;
}

const int maxn = 1003;
int x[maxn], y[maxn];
ll diff[maxn][maxn],diag[maxn];


int abs_(int x) {
    return x > 0 ? x : -x;
}
int main() {
    int T;
    scanf("%d", &T);
    while (T--) {
        int n, p;
        ll V;
        scanf("%d%lld%d", &n, &V, &p);
        for (int i = 0; i < n; i++) scanf("%d", x + i);
        for (int i = 0; i < n; i++) scanf("%d", y + i);
        for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            diff[i][j] = Pow(abs_(x[i] - y[j]), p);
        int ans = 0;
        for (int i = 0; i < n; i++) {
            int di = 0;
            for (int xi = 0, yi = i; xi < n && yi < n; xi++, yi++)
                diag[di++] = diff[xi][yi];
           
            ans += count(diag, di, V);
        }
        for (int i = 1; i < n; i++) {
            int di = 0;
            for (int xi = i, yi = 0; xi < n && yi < n; xi++, yi++)
                diag[di++] = diff[xi][yi];
           
            ans += count(diag, di, V);
        }
        printf("%d\n",ans);
    }
}


```
