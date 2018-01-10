---
title: 'POJ 3468 [A Simple Problem with Integers] 题解'
comments: true
categories: ACM
tags:
  - BIT
  - data structures
abbrlink: 8b465320
date: 2018-01-09 17:30:31
---

# 题目大意
给出长度为N的数组，Q次询问，每次询问有两种操作：
1. 查询某个区间的和。
2. 将某个区间的数全部加上x。

<!-- more -->

# 题目分析
要同时支持区间更新和区间查询（求和）两种操作，考虑树状数组。

但我们知道，树状数组只支持区间查询和点更新，或者区间更新和点查询。怎么样才能做到同时更新区间以及对区间求和呢？

我们先来考察区间更新后，前缀和的增量。记前缀和为S(i)，更新区间[l,r]后前缀和为S'(i).

$$\begin{eqnarray}S'(i)= \begin{cases} S(i), &i<l\cr S(i) + x(i-l+1), &l\leqslant i\leqslant r \cr S(i)+x(r-l+1), &i>r\end{cases} \end{eqnarray} $$

留意到i<l时，增量为0，无需处理；i>r时，增量为常数，像普通BIT那样进行一次点更新即可。

难就难在， $l\leqslant i\leqslant r$时，增量是一个关于i的一次函数。

如前面所说，普通BIT进行一次点更新只能导致前缀和增加一个常数。既然如此，我们把上面这个一次函数的增量拆成两项，用两个BIT维护：

$$\Delta S(i)=xi + x(1-l) $$
一次项系数为x，常数项为x(1-l)，分别对应BIT1和BIT0的增量值。

这样，记BIT0前缀和为s0，BIT1前缀和为s1，则总的前缀和就是`s0 + s1 * x`

用前缀和来表示原数组的每一个值，然后区间求和就可转化为左右端点的前缀和差值，即点查询+区间更新的模式。

然后，更新的时候，分别对BIT0和BIT1进行区间更新即可（实质上是更新两个端点）。

# source code 
```c++
#include <cstdio>
typedef long long ll;
const int maxn = 100010;
ll bit[2][maxn];
int n,q;

int lowbit(int x) {
    return x & -x;
}

ll sum(int x) {
    ll s0 = 0, s1 = 0;
    for (int i = x; i > 0; i -= lowbit(i)) s0 += bit[0][i], s1 += bit[1][i];
    return s0 + s1 * x;
}

void add(int ti, int x, ll val) {
    for (int i = x; i <= n; i += lowbit(i)) bit[ti][i] += val;
}

void update(int l, int r, ll val) { // [l, r]
    add(0, l, val * (1 - l));
    add(1, l, val);
    add(0, r + 1, val * r);
    add(1, r + 1, -val);
}


int main() {
    scanf("%d%d", &n, &q);
    for (int i = 1; i <= n; i++) {
        int x;
        scanf("%d", &x);
        update(i, i, x);
    }
    while (q--) {
        char s[10];
        int l, r, x;
        scanf("%s%d%d", s, &l, &r);
        if(s[0] == 'Q'){
            printf("%lld\n", sum(r) - sum(l - 1));
        }
        else {
            scanf("%d", &x);
            update(l, r, x);
        }
    }
}
```