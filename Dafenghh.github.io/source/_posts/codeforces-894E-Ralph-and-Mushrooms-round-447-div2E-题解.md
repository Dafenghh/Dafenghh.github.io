---
title: 'codeforces 894E [Ralph and Mushrooms] #round 447 div2E 题解'
comments: true
categories: ACM
tags:
  - dp
  - graphs
  - scc
abbrlink: 2fde12e8
date: 2017-11-27 19:39:47
---

## 题目大意
一个有向图，n个点，m条边，每条边初始有w[i]个蘑菇。有可能存在自环或重边。从点s出发，任意移动，当第一次经过一条边时，能采集w[i]个蘑菇；第二次经过这条边时，能采集w[i] - 1个蘑菇；第三次经过时，能采集w[i] - 1 - 2个；第四次则为w[i] - 1 - 2 - 3个，依次类推。当可采集蘑菇数变成0或负数后，仍能从这条边经过，但不会获得蘑菇。求能采集到的蘑菇的最大数量。

<!-- more -->

## 题目分析
经过强连通分量(scc)分解后，并缩成一个DAG。显然每个scc上，可以任意次数遍历这个scc包含的边，直至无蘑菇可采。然后scc之间的边，最多只能经过一次。
对那些可以无限遍历的边，我们先用二分求出总蘑菇数，然后汇总到所在的scc中。
最后应用DAG上的动态规划，算出从点s出发, 能采集到的最大蘑菇数。


## source_code
```c++
#include <iostream>
#include <vector>
#include <cstring>
#include <algorithm>
#include <map>
#include <utility>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
int V;
const int maxv= 1000010;
vector<int> G[maxv], rG[maxv], vs;
bool used[maxv];
int cmp[maxv];
void add_edge(int from, int to){
    G[from].push_back(to);
    rG[to].push_back(from); 
}
void dfs(int v){
    used[v] = true;
    for(int  i = 0; i < G[v].size(); i++){
        if (!used[G[v][i]]) dfs(G[v][i]);
    }
    vs.push_back(v);
}

void rdfs(int v, int k){
    used[v] = true;
    cmp[v] = k;
    for (int i = 0; i < rG[v].size(); i++){
        if (!used[rG[v][i]]) rdfs(rG[v][i], k);
    }
}

int scc() {
    memset(used, 0, sizeof(used));
    vs.clear();
    for (int v = 1; v <= V; v++){
        if (!used[v]) dfs(v);
    }
    memset(used, 0 , sizeof(used));
    int k = 0;
    for (int i = vs.size() - 1; i >= 0; i--){
        if(!used[vs[i]]) rdfs(vs[i], k++);
    }
    return k;
}

const int sn = 22000;
ll sum[sn], accSum[sn];

void init_sum() {
    sum[0] = accSum[sn] = 0;
    for (int i = 1; i < sn; i++) sum[i] = sum[i - 1] + i;
    for (int i = 1; i < sn; i++) accSum[i] = accSum[i - 1] + sum[i];
}

ll calWeight(int x) {
   int k = upper_bound(sum, sum + sn, x) - sum - 1;
   return (ll)(k+1) * x - accSum[k]; 
} 

ll shrink_vertex_weight[maxv], dp[maxv];
int sV;
vector<int> shrink_graph[maxv];
map<P, ll> shrink_edge_weight;
struct edge{int from, to, weight;};
edge edges[maxv];


void update(ll &a, ll b) {
    if (b > a) a = b;
}

ll f(int v) {
    if (dp[v] > -1) return dp[v];
    ll reward = 0;
    for (auto i: shrink_graph[v]) {
        update(reward, shrink_edge_weight[P(v, i)] + f(i));
    }
    return dp[v] = shrink_vertex_weight[v] + reward;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    init_sum();
    int n, m;
    cin >> n >> m;
    V = n;
    for (int i = 0; i < m; i++) {
        int from, to, weight;
        cin >> from >> to >> weight;
        add_edge(from, to);
        edges[i] = edge{from, to, weight};
    }
    int start;
    cin >> start;
    sV = scc();
    for (int i = 0; i < m; i++) {
        int v1 = edges[i].from, v2 = edges[i].to;
        if (cmp[v1] == cmp[v2]) {
            shrink_vertex_weight[cmp[v1]] += calWeight(edges[i].weight);
        }
        else {
            update(shrink_edge_weight[P(cmp[v1], cmp[v2])], edges[i].weight);
            shrink_graph[cmp[v1]].push_back(cmp[v2]);
        }
    }
    memset(dp, -1, sizeof(dp));
    cout << f(cmp[start]) << endl;
}
```