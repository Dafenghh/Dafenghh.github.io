---
title: 'POJ 1990 [MooFest] 题解'
comments: true
date: 2017-12-31 00:23:57
categories: ACM
tags:
  - data structures
  - BIT
---
# 题目大意
N头牛排成一排，现在给出它们的坐标x[i]和听觉阈值v[i]. 两头牛i和j之间谈话的音量为`max(v[i], v[j]) * dist(i, j)` dist表示两者距离。求所有N*(N-1)对牛谈话音量的总和。

<!-- more -->

# 题目分析
按v[i]降序排序。然后用BIT求出每一头牛和后面的牛的距离的和即可。（我用了两个BIT，一个维护累计和，一个维护个数）


# source code
```c++
#include <cstdio>
#include <algorithm>
using namespace std;
const int maxn = 20200;
typedef long long ll;
int n = 20000;


int lowbit(int x) {
    return x & -x;
}

ll bit[maxn];

ll sum(int i) {
    ll res = 0;
    for (; i; i -= lowbit(i)) {
        res += bit[i];
    }
    return res;
}

void add(int i, ll x) {
    for (; i <= n; i += lowbit(i)) {
        bit[i] += x;
    }
}

ll bit2[maxn];

ll sum2(int i) {
    ll res = 0;
    for (; i; i -= lowbit(i)) {
        res += bit2[i];
    }
    return res;
}

void add2(int i, ll x) {
    for (; i <= n; i += lowbit(i)) {
        bit2[i] += x;
    }
}

struct cow {
    int v, x;
    bool operator < (const cow & b) const {
        return v > b.v;
    }
}a[maxn];


int main() {
    int N;
    scanf("%d", &N);
    for (int i = 0; i < N; i++) scanf("%d%d", &a[i].v, &a[i].x), add(a[i].x, a[i].x), add2(a[i].x, 1);
    sort(a, a + N);
    ll ans = 0;
    for (int i = 0; i < N - 1; i++) {
        add(a[i].x, -a[i].x);
        add2(a[i].x, -1);
        ll dist_tot = sum2(a[i].x) * a[i].x - sum(a[i].x) + sum(n) - sum(a[i].x) - (sum2(n) - sum2(a[i].x)) * a[i].x;
        ans += dist_tot * a[i].v;
    }
    printf("%lld\n", ans);
}

```