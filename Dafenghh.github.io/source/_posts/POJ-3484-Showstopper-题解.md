---
title: 'POJ 3484 [Showstopper] 题解'
comments: true
categories: ACM
tags:
  - binary search
  - 按行输入
abbrlink: b9568341
date: 2017-11-28 22:44:04
---

## 题目大意
给定若干等差数列，初项x，公差z，末项不大于y（均为正整数）。在所有数列有，有且仅有一个数的出现次数是奇数，求这个数以及它出现的次数。

<!-- more -->


## 题目分析
初看此题，真的毫无思路。虽然知道实在《挑战程序设计竞赛》的二分专题中，肯定使用二分算法，但怎么联系到二分上去呢？

有且只有一个奇数，这就是突破口。对解空间中的所有数，统计它们在数列中的出现次数，记为c[i]，然后对c[i]求个前缀和，那么在答案点x前，前缀和均为偶，答案点x及之后，前缀和均为奇。由此得到二分的单调性，得解。

有点小插曲，这道题的输入方式很麻烦……最讨厌按行输入了。WA几次后以为是输入有问题，其实是算法部分写错了细节。统计区间的数在数列的出现次数时，左端应该向上取整……

代码比较累赘，特别是统计的时候，可以写精简些。

## source code
```c++
#include <cstdio>
#include <cstring>
#include <cctype>
#include <vector>
#include <algorithm>
using namespace std;
typedef long long ll;

struct Tuple {
    ll x, y, z;
    Tuple(ll x = 0, ll y = 0, ll z = 0):x(x), y(y), z(z){}
};

vector<Tuple> vec;

ll count_(ll L, ll R, Tuple t) { //[L, R]
    ll x = t.x, y = t.y, z = t.z;
    if (R < x || L > y) return 0;
    if (L < x) L = x;
    if (R > y) R = y;
    ll res =  (R - x ) / z - (L - x) / z + 1;
    if ((L - x) % z) res--;
    return res;
}

void solve() {
    /*printf("-------------\n");
    for (ll i = 0; i < vec.size(); i++) {
        printf("%lld %lld %lld\n", vec[i].x, vec[i].y, vec[i].z);
    }
    printf("-------------\n");*/
    if (vec.empty()) return;
    ll L = vec[0].x, R = vec[0].y;
    for (ll i = 1; i < vec.size(); i++) {
        L = min(L, vec[i].x);
        R = max(R, vec[i].y);
    }
    L--;

    ll sum = 0;
    for (ll i = 0; i < vec.size(); i++) {
        sum += count_(L, R, vec[i]);
    }
    if (sum % 2 == 0) {
        printf("no corruption\n");
        return;
    }
    
    while (L + 1 < R) {
        ll mid = (L + R) / 2;
        ll sum = 0;
        for (ll i = 0; i < vec.size(); i++) {
           sum += count_(L, mid, vec[i]);
        }
        if (sum % 2 == 0) L = mid;
        else R = mid;
    }
    sum = 0;
    for (ll i = 0; i < vec.size(); i++) {
        sum += count_(R, R, vec[i]);
    }
    
    ll ans = (ll)R;
    printf("%I64d %I64d\n", ans, sum);
}

char s[1000];

void work() {
    ll x = 0, y = 0, z = 0;
    sscanf(s, "%I64d %I64d %I64d", &x, &y, &z);
    if (!x) return;
    vec.clear();
    vec.push_back(Tuple(x, y, z));
    memset(s, 0, sizeof(s));
    while (gets(s), *s)
    {
        sscanf(s, "%I64d %I64d %I64d", &x, &y, &z);
        vec.push_back(Tuple(x, y, z));
        memset(s , 0 , sizeof(s));
    }
    solve();
}


int main() {
    while (gets(s)) work();
}


```