---
title: >-
  codeforces gym 101612 H [Hidden Supervisors] (ICPC 2017-2018 NEERC Northern
  Subregional Contest St Petersburg November 4 2017) 题解
comments: true
categories: ACM
tags:
  - trees
  - greedy
abbrlink: 2cb42504
date: 2017-11-29 22:38:43
---
## 题目描述
gym 101612 H题
链接： [gym](http://codeforces.com/gym/101612)


给定若干棵树，其中一棵的根结点为1，现在把这所有的树合并成一棵根结点为1的树，并且要使这棵树中能够组成的(a,b)(a是b的父结点)的组数最大。


<!-- more -->

## 题目分析

先对每棵树做下DFS，用贪心的方式分好组。这棵树的根结点要么分好组，要么没分好。对所有不等于1的根结点，如果它已经分好组，就直接连到1上面；如果没分好组，就连到结点1的树未分组的一个结点上。对没分好组的根结点，用下贪心策略，对应的树的未分组结点越多的根结点越优先连接。

就这样简单的贪心策略，完成此题……想复杂了，并且细节上犯了弱智错误，debug很久。（根结点的分组居然写在了countUnmatchedNodesForAllTrees函数之前）。



## source code
```c++
//#define LOCAL

#include <cstdio>
#include <set>
#include <vector>
#include <queue>
#include <utility>
#include <algorithm>
using namespace std;
const int maxn = 100010;
int n;
int p[maxn], rt[maxn];
vector<int> G[maxn];
typedef pair<int, int> P;
bool matched[maxn];
int cnt = 0;
vector<int> unmatched_nodes[maxn];

set<int> S;

void addUnmatchedNodes(int v){
    int u = rt[v];
    unmatched_nodes[u].push_back(v);
}

void addUnmatchedNodesToRoot(int v){
    for (auto i: unmatched_nodes[v]){
        S.insert(i);
    }
    //unmatched_nodes[v].clear();
}

bool Match(int v, int root) {
    rt[v] = root;
    bool res = false;
    for (auto i: G[v]) {
        if (!Match(i, root)){
            if (!res){
                matched[v] = true;
                matched[i] = true;
                res = true;
                cnt++;
            }
        }
    }
    return res;
}

void countUnmatchedNodesForAllTrees() {
    for (int i = 1; i <= n; i++) {
        if (!matched[i]) {
            addUnmatchedNodes(i);
        }
    }
}

int main() {
    #ifndef LOCAL
    freopen("hidden.in", "r", stdin);
    freopen("hidden.out", "w", stdout);
    #endif
    scanf("%d", &n);

    for (int i = 2; i <= n; i++) {
        scanf("%d", p + i);
        G[p[i]].push_back(i);
    }
    Match(1, 1);
    queue<int> que0;
    priority_queue<P> que1;//que0: matched que1: unmatched
    for (auto i: G[0]) {
        Match(i, i);
    }
    countUnmatchedNodesForAllTrees();
    for (auto i: G[0]) {
        if (matched[i]) {
            que0.push(i);
        } 
        else que1.push(P(unmatched_nodes[i].size(),i));
    }

    
    addUnmatchedNodesToRoot(1);
    
    while (!que0.empty()) {
        int u = que0.front(); que0.pop();
        p[u] = 1;
        addUnmatchedNodesToRoot(u);
    }

    while (!que1.empty()) {
        int u = que1.top().second; que1.pop();
        if (!S.empty()){
            int v = *S.begin();
            p[u] = v;
            cnt++;
            addUnmatchedNodesToRoot(u);
            S.erase(u);
            S.erase(v);
        }
        else {
            p[u] = 1;
            addUnmatchedNodesToRoot(u);
        }
    }
    printf("%d\n", cnt);
    for (int i = 2; i <= n; i++) printf("%d%c", p[i], (i == n?10:32));
}
```