---
title: 'codeforces 908F [New Year and Rainbow Roads] (good bye 2017) 题解'
comments: true
categories: ACM
tags:
  - greedy
abbrlink: '8234636'
date: 2017-12-30 15:05:48
---
# 题目大意
给定一维直线上的N个点，每个点有一个颜色标记（红、绿、蓝），现在要连边，一条边的代价为两点距离。现求最小总代价，使红、绿点连通，并且蓝、绿点连通。

<!-- more -->

# 题目分析
考虑到，红、蓝点连线并无意义，所以可忽略；另外考虑假如出现了连续三个点，颜色分别为红绿红，那么红和红之间连线一定不必红-绿-红这样连线优。所以，以绿点为分割点，分成若干个区间单独考虑即可。

对于两个连续的绿点之间的区间，有两种连线方式：（1）连接两个绿点，再以从这两个绿点为端点，连出两条线，分别连接中间的红点和蓝点，接着分别去掉这两条线的最长一个区间；（2）直接从这两个绿点为端点，连出两条线，分别连接中间的红点和蓝点。

分别统计两种连线方式的代价，选最优即可。

代码写得很丑，要看漂亮代码请戳这：[by aaaaajack](http://codeforces.com/contest/908/submission/33789715)


# source code
```c++
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>
using namespace std;
typedef long long ll;
vector<ll> v0, v1, v2;
ll ans;
void add(vector<ll> &v){
    if (v.empty()) return;
    ans += max(0LL, v0.front() - v.front()) + max(0LL, v.back() - v0.back());
}

vector<ll>::iterator LB(vector<ll> &v, ll x) {
    return lower_bound(v.begin(), v.end(), x);
}

ll calc(ll l, ll r) {
    if (LB(v1, l) == LB(v1, r) && LB(v2, l) == LB(v2, r)) return r - l;
    ll x = l, max_interval = 0;
    for (auto it = LB(v1, l); it != v1.end() && (*it) < r; it++) {
        max_interval = max(max_interval, (*it) - x);
        x = *it;
    }
    max_interval = max(max_interval, r - x);
    ll ans1 = 3 * (r - l) - max_interval;
    x = l; max_interval = 0;

    for (auto it = LB(v2, l); it != v2.end() && (*it) < r; it++) {
        max_interval = max(max_interval, (*it) - x);
        x = *it;
    }
    max_interval = max(max_interval, r - x);
    ans1 -= max_interval;
    return min(ans1, 2 * (r - l));
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    ll N;
    cin >> N;
    for (ll i = 0; i < N; i++) {
        ll x;
        string s;
        cin >> x >> s;
        if (s[0] == 'G') v0.push_back(x);
        else if (s[0] == 'B') v1.push_back(x);
        else v2.push_back(x);
    }
    
    if (v0.size() <= 1) {
        if (!v1.empty()) ans += v1.back() - v1.front();
        if (!v2.empty()) ans += v2.back() - v2.front();
    }
    else {
        add(v1);
        add(v2);
        for (ll i = 0; i < v0.size() - 1; i++) {
            ans += calc(v0[i], v0[i + 1]);
        }
    }
    cout << ans << endl;
}

```