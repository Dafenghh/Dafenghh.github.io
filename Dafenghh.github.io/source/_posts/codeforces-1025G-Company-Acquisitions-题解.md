---
title: codeforces 1025G Company Acquisitions 题解
comments: true
categories: ACM
tags:
  - math
  - constructive algorithms
abbrlink: d05b63c9
date: 2018-09-11 17:07:52
---
# 题目大意
n家公司，每个公司有两种状态，独立或者附属于某一独立公司。每天发生这样一次操作：等概率选择两个独立公司A和B，等概率指定A吞并B,或者B吞并A。假设A吞并B，那么B就成为A的附属公司，然后原来附属B的公司全部独立。给定初始每个公司的状态，求期望多少天后，只有一家独立公司。
<!-- more -->
# solution
超有意思的数学（构造）题……
一直在想DP做法，但这个状态没办法表示。所以要转化思路。
设一个有k个附属公司的独立公司的potential值为$2^k-1$，则可证明，一次操作后，potential和的增量的期望为1. 然后这就简单了，将终态和初始态的potential相减就行了。
# source code
```c++
#include <bits/stdc++.h>
using namespace std;
const int Mod = 1e9+7;
typedef long long ll;
ll mod_pow(ll x, ll n) {
    ll res = 1;
    for (; n > 0; n >>= 1) {
        if (n & 1) res = res * x % Mod;
        x = x * x % Mod;
    }
    return res;
}
int cnt[600];
void add(int &a, ll b) {
    a = (a + b) % Mod;
}
int main() {
    int n;
    scanf("%d", &n);
    int ans = 0;
    add(ans, mod_pow(2, n - 1) - 1);

    for (int i = 1; i <= n; i++) {
        int x;
        scanf("%d", &x);
        if (x != -1) ++cnt[x];
    }
    for (int x = 1; x <= n; x++) {
        if (cnt[x] > 0) add(ans, 1 - mod_pow(2, cnt[x]));
    }
    add(ans, Mod);
    printf("%d\n", ans);
}
```