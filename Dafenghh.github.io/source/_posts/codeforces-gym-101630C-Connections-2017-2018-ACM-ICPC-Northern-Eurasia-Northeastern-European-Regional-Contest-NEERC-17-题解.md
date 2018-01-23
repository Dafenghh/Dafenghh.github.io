---
title: >-
  codeforces gym 101630C [Connections] 2017-2018 ACM-ICPC, Northern Eurasia
  (Northeastern European Regional) Contest (NEERC 17) 题解
comments: true
categories: ACM
tags:
  - graphs
  - dfs and similar
abbrlink: a35a5595
date: 2018-01-23 19:01:49
---
# 题目大意
给定一个强连通的有向图，n个点，m条边；要求，去掉m-2n条边，使剩下的图仍然强连通。


<!-- more -->


# 题目分析
采用DFS，将需要保留下的边打上记号。然后任意输出m-2n条不需要保留的边即可。

哪些边需要保留呢？

DFS前进的那些边（即搜索树中的边，“实边”）一定要保留，至于虚边，则保留得越少越好。

所以优先从搜索树的靠叶子端取虚边。比如DFS依次经过1->2->3->4, 我们就先从4开始取回去的边，回去得越前越好，即如果同时存在(4,3)和(4,2）就取（4，2）,然后回到结点2，再取一条(2,1)就能构成一个环，也即强连通的子图。

具体实现的话，dfs返回当前访问子树能够回去的最前结点（有点类似tarjan的lowlink）。对一个结点来说，所有虚边只需考虑最优那条（即返回的点index越小越好）。然后拿它跟子树返回结果的最小值（代表子树能够回去的最前结点）比较，如果这条虚边更有，就连上。


P.S. 1A这题，很开心！

# source code
```c++
#include <map>
#include <cstdio>
#include <cstring>
#include <vector>
#include <utility>
#include <algorithm>
using namespace std;
int n,m;
const int maxn = 100020;
vector<int> G[maxn];
typedef pair<int, int> P;
map<P, int> M;
bool used[maxn];
int index[maxn], cnt;
int dfs(int v) {
    index[v] = cnt++;
    used[v] = true;
    int bv = v, nbi = index[v];
    for (auto i: G[v]) {
        if (!used[i]) {
            M[P(v,i)] = 1;
            nbi = min(nbi, dfs(i));
        }
        else if (index[i] < index[bv]) bv = i;
    }
    if (index[bv] < nbi) {
        M[P(v, bv)] = 1;
    }
    return min(nbi, index[bv]);
}
int main() {
    int T;	
    scanf("%d", &T);
    while (T--) {
      scanf("%d%d", &n, &m);
      M.clear();
      cnt = 0;
      memset(used, 0, sizeof(used));
      memset(index, 0, sizeof(index));
      for (int i = 1; i <= n; i++) G[i].clear();
      for (int i = 0; i < m; i++) {
              int x,y;
              scanf("%d%d", &x, &y);
              G[x].push_back(y);
              M[P(x,y)] = 0;
      }
      
      dfs(1);
      int tot = 0;
      for (auto i: M) {
          if (!i.second) {
              tot++;
              printf("%d %d\n", i.first.first, i.first.second);
              if (tot == m - 2 * n) break;
          }
      }
      
      
    }
}

```