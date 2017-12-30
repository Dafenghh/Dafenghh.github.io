---
title: 'codeforces 343D [Water Tree] 题解'
comments: true
date: 2017-12-30 14:26:56
categories: ACM
tags:
  - trees
  - dfs and similar
  - data structures
---
# 题目大意
给定一棵树，可以进行以下三种操作：
1. 选择一个结点，使这个结点对应的子树全部充满水。
2. 选择一个结点，除去它和它所有的祖先的水。
3. 选择一个结点，查询是否有水。

# 题目分析

# source code
```c++
#include <set>
#include <vector>
#include <cstdio>
using namespace std;
const int maxn = 500010;
vector<int> G[maxn];
int L[maxn], R[maxn], fa[maxn];
int cnt,n ,q;
set<int> S;
void dfs(int v, int par = -1) {
    L[v] = ++cnt;
    for (auto i:G[v]) {
        if (i != par) {
            fa[i] = v;dfs(i, v);
        }
    }
    R[v] = cnt + 1;
    if (R[v] == L[v] + 1) S.insert(L[v]);
}

bool empty(int v) {
    auto it = S.lower_bound(L[v]);
    return it != S.end() && (*it) < R[v];
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n - 1; i++) {
        int x,y;
        scanf("%d%d", &x, &y);
        G[x].push_back(y);
        G[y].push_back(x);
    }
    dfs(1);
    scanf("%d", &q);
    while (q--) {
        int c, v;
        scanf("%d%d", &c, &v);
        if (c == 3) puts((empty(v) ? "0" : "1"));
        else if (c == 1) {
            if (fa[v] && empty(fa[v])) S.insert(L[fa[v]]);
            S.erase(S.lower_bound(L[v]), S.lower_bound(R[v]));
        }
        else S.insert(L[v]);
    }
}
```