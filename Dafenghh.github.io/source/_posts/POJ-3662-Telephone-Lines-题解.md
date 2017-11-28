---
title: 'POJ 3662 [Telephone Lines] 题解'
comments: true
categories: ACM
tags:
  - binary search
  - graphs
abbrlink: e84ff597
date: 2017-11-27 14:21:30
---

## 题目大意
一个无向图，N的点，P条边，给出每条边的长度，定义一条路径的代价为省略路径上的K条边后剩余边的边长最大值。求点1到点N代价最小的路径的代价。

<!-- more -->

## 题目分析
“最小化最大值”的题目，考虑二分。
二分时，设当前设定代价为k，然后考虑在代价为k的约束下能否找到一条可行路径。在寻找路径时，碰到一条边长大于k的边，那么连上这条边需要省略它。显然，一条可行路径就是指路径上所有被省略的边数量不超过K。

那么就可以转化一下，边长大于k的边权值为1，边长小于等于k的边权值为0.然后用dijkstra计算1到n的最短路，如果答案不大于K，说明被省略的边数量不超过K.

## source code
```c++
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>
#include <utility>
#include <functional>
using namespace std;
const int INF = 20344;
const int MAX_V = 1500;
struct edge {
    int to, cost;
    edge(int to, int cost):to(to),cost(cost){}
};
typedef pair<int, int> P;
int V;
vector<edge> G[MAX_V];
int d[MAX_V];

int dijkstra(int s, int t, int k) {
    priority_queue<P, vector<P>, greater<P> > que;
    fill(d + 1, d + V + 1, INF);
    d[s] = 0;
    que.push(P(0, s));
    while (!que.empty()) {
        P p = que.top(); que.pop();
        int v = p.second;
        if (d[v] < p.first) continue;
        for (int i = 0; i < G[v].size(); i++) {
            edge e = G[v][i];
            if (d[e.to] > d[v] + (e.cost > k ? 1 : 0)) {
                d[e.to] = d[v] + (e.cost > k ? 1 : 0);
                que.push(P(d[e.to], e.to));
            }
        }
    }
    return d[t];
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int N, P, K;
    cin >> N >> P >> K;
    V = N;
    for (int i = 0; i < P; i++){
        int from, to, cost;
        cin >> from >> to >> cost;
        G[from].push_back(edge(to, cost));
        G[to].push_back(edge(from, cost));
    }
    int L = -1, R = 1000010;
    if (dijkstra(1, N, R) == INF) {
        cout << -1 << endl;
        return 0;
    }
    while (L + 1 < R) {
        int mid = (L + R) >> 1;
        if (dijkstra(1, N, mid) <= K) R = mid;
        else L = mid;
    }
    cout << R << endl;
}
```

