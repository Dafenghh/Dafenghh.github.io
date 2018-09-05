---
title: CodeChef - GENPERM Generating A Permutation 题解
comments: true
categories: ACM
tags:
  - constructive algorithms
abbrlink: '75299267'
date: 2018-05-09 21:15:57
---
# 题目大意
对一个排列P，[p1, p2, ..., pN-1, pN], 定义 f(P) = max(p1, p2) + max(p2, p3) + ... + max(pN-1, pN)

给出N，K。构造排列P， 使f(P) = K. 如果不存在，就输出-1.

# solution
f(P)的值是N-1个数的和。而原排列中的数，最多贡献两次答案。

考虑排列是正序时，f(P)取到最小值，为2+3+...+N。也即2到N的数，每个数贡献1次。
最大值应该是在最大的那些数都贡献2次时取到。

怎么让最大那个数贡献2次？只需要不把它放两端就可以了。

另外注意到，一个大的数a多贡献一次的同时，就会剥夺另外一个较小的数b的贡献机会，这时候就会使答案增加a-b.

所以这道题的构造方法就是，设a为下一个贡献多一次的数，b为下一个被剥夺贡献机会的数。考虑当前的和，如果加上b-a比答案大，我们就让b大一些，直到满足条件。然后把刚刚考虑过的b依次append到答案数组，再append a就行。

# source code

```c++
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5+10;

typedef long long ll;
int main() {
    int T;
    scanf("%d", &T);
    while (T--) {
        ll k;
        int n;
        scanf("%d%lld", &n, &k);
        if (n == 1) {
            if (k == 0) {
                printf("1\n");
            }
            else {
                printf("-1\n");
            }
            continue;
        }
        ll x = 0;
        for (int i = 2; i <= n; i++) x += i;
        if (k < x) {
            printf("-1\n");
            continue;
        }
        int l = 1, r = n;
        vector<int> vec;
        while (l + 1 < r) {
            if (x + r - l - 1 <= k) {
                x += r - l - 1;
                vec.push_back(l++);
                vec.push_back(r--);
            }
            else {
                vec.push_back(l++);
            }
        }

        for (int i = l; i <= r; i++) vec.push_back(i);
        if (x != k || vec.size() != n) {
            printf("-1\n");
            continue;
        }
        for (int i = 0; i < n; i++) {
            printf("%d%c", vec[i], " \n"[i==n-1]);
        }
    }
}

```
