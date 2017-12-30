---
title: 'codeforces 343D [Water Tree] 题解'
comments: true
categories: ACM
tags:
  - trees
  - dfs and similar
  - data structures
abbrlink: a28d78f2
date: 2017-12-30 14:26:56
---
# 题目大意
给定一棵树，可以进行以下三种操作：
1. 选择一个结点v，使v对应的子树全部充满水。
2. 选择一个结点v，除去v和v所有的祖先的水。
3. 选择一个结点v，查询v是否有水。

<!-- more -->

# 题目分析
看完《挑战程序设计竞赛》的线段树章节后，搜了下codeforces的线段树题目来做，然后就搜到这题。这题做了很长时间哎，而且犯了不少很囧的错误。

首先看错了题，以为第二种操作是除去子树的水，所以很轻巧的敲了个BIT，跑起来不对才发现自己弄错了题意……

于是删掉代码重写。想到要更新所有祖先结点，岂不是要用树链剖分？参考了下cf的Tutorial，原来使用一个很巧妙的转化，就能变成普通的线段树问题。

我思考这题的时候，总是想着加水就是染成1，去水就是染成0，所以这两个操作分别对应子树的更新和路径的更新，这么做起来就很麻烦。Tutorial将思路倒转一下，便是去水只需要在v（最末端结点）上打个标记，加水是将子树上的所有标记去掉，查询某点是否有水是看这个点对应子树是否有标记，只要有一个标记，说明这点没水；没标记才表示有水。

而初始状态，我们将所有叶子结点打上标记，就可表示全没水的状态。

这样一来，确定dfs序之后，找出每个结点对应子树区间[L,R], 然后使用std::set即可实现这个算法，还很简单，都不需要线段树了。

cf上的前排代码求dfs时，将`R[v] = cnt + 1`写成`R[v] = ++cnt`

我一开始很疑惑，这样写有什么区别，因为++后，就相当于对每个子树在末端新建了个虚拟的占位位置。

其实没区别。只是前排代码这么写，那么这一句`S.erase(S.lower_bound(L[v]), S.lower_bound(R[v]))` 的后半部分`S.lower_bound(R[v])`既可以用`lower_bound`, 又可以用`upper_bound`, 我的代码就只能用`lower_bound`




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