---
title: 'HDU 5249 [KPI] 题解'
comments: true
date: 2018-01-17 15:59:57
categories: ACM
tags:
  - BIT
  - data structures
---
# 题目大意
n次操作，有3种操作的类型：入队、出队、查询队列中的中位数。

<!-- more -->

# 题目分析
区间第K大的简单版本。用权值线段树可以轻松解决。这里尝试使用一下树状数组。这里搞明白树状数组的原理后，findK函数可以写得很清晰。

稍微解释一下findK函数，从x=1<<16（值的上界）开始定位答案，判定bit[]数组的值来定位答案位于权区间的左半部分还是右半部分。反复执行这个过程，逐步缩小区间得到答案。


# source code
```c++
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
const int maxn = 10020;
int tot, bit[maxn];
void update(int i, int x) {
    for (; i <= tot; i += i & -i) bit[i] += x;
}

int findK(int k) {
    int ans = 0;
    for (int x = (1 << 16); x; x >>= 1) {
        if (ans + x < tot && bit[ans + x] < k) {
            k -= bit[ans += x];
        }
    }
    return ans;
}
char Q[maxn];
int a[maxn], b[maxn], n;
void init() {
    n = 0;
    memset(bit, 0, sizeof(bit));
}

int main() {
    int qn, ti = 0;
    while (scanf("%d", &qn) != EOF) {
        init();
        ++ti;
        printf("Case #%d:\n", ti);
        for (int i = 0; i < qn; i++) {
            char s[10];
            scanf("%s", s);
            Q[i] = s[0];
            if (Q[i] == 'i') {
                scanf("%d", &a[n]);
                b[n] = a[n];
                n++;
            }
        }
        sort(b, b + n);
        tot = n;
        for (int i = 0; i < n; i++) a[i] = lower_bound(b, b + n, a[i]) - b + 1;
        int ql = 0, qr = 0;
        for (int i = 0; i < qn; i++) {
            if (Q[i] == 'i') {
                update(a[qr++], 1);
            }
            else if (Q[i] == 'o') {
                update(a[ql++], -1);
            }
            else {
                printf("%d\n", b[findK((qr - ql + 2)/ 2)]);
            }
        }
    }
}
```