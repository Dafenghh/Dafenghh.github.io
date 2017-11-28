---
title: 'POJ 3276 [Face The Right Way] 题解'
comments: true
date: 2017-11-28 20:22:37
categories: ACM
tags:
- 区间处理
- 反转
---

## 题目大意
n头牛排成一行，有的牛面朝前，有的牛面朝后，每一次操作可以使连续的K头牛改变方向；求一个K，使得操作次数最少。输出K以及最少的操作次数。当有多个K满足条件时，输出最小的K。


<!-- more -->


## 题目分析
对一个区间来说，多次进行反转操作是没有意义的；另外反转的顺序对结果是没有影响的。所以这道题只需要对所有的可操作区间（即长度为K的区间）考虑是否需要反转。

考虑最左边的牛，当它面朝前时无需反转，当它面朝后时，就反转[1, K]区间一次。然后继续考虑第二头牛即可。

反转的时候不必每头牛都操作一次，只需用一个turns来记录当前区间的反转次数，考虑的下一头牛的状态就由它本身状态+turns的值来决定。随着区间往右移动，我们用区间左右更改的地方来更新turns即可。复杂度为`O(n ^ 2) `


## source code
```c++
#include <cstdio>
#include <cstring>
const int maxn = 5500;
const int INF = 100243535;
int a[maxn], f[maxn], cnt[maxn];
int main() {
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        char s[10];
        scanf("%s", s);
        a[i] = (s[0] == 'F' ? 0 : 1);
    }
    for (int k = 1; k <= n; k++) {
        memset(f, 0, sizeof(f));
        int turns = 0;
        for (int i = 0; i < n; i++) {
           
            if ((a[i] + turns) % 2 == 1) {
                if (i + k > n) {
                    cnt[k] = INF;
                    break;
                }
                f[i] = 1;
                cnt[k]++;
                turns++;
            }
            if (i - k + 1 >= 0) turns -= f[i - k + 1];
           
        }   
    }
    int ansK = 1;

    for (int k = 2; k <= n; k++) if (cnt[k] < cnt[ansK]) ansK = k;
   
    printf("%d %d\n", ansK, cnt[ansK]);

}
```
