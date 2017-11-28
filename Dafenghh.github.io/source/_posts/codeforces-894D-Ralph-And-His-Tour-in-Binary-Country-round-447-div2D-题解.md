---
title: 'codeforces 894D [Ralph And His Tour in Binary Country] round 447 div2D 题解'
comments: true
categories: ACM
tags:
  - trees
  - data structure
abbrlink: 2d71363f
date: 2017-11-26 09:56:29
---

## 题目大意
给出一棵结点数为n的完全二叉树以及所有边的长度，m次查询，每次查询输入两个正整数A和H，A为起始点，任选树上一个结点（可以选择A本身）作为终点来构成一段旅程，定义这段旅程的快乐值为H-起点到终点的路径长度。求所有快乐值为正的旅程的快乐值的和。
<!-- more -->

## 题目分析
完全二叉树的性质在这道题要好好利用一下。因为是完全二叉树，所以树的高度为O(log n)级别。这样，我们对每一个结点，建一个数组（vector），记录从这个结点出发往它的子树上所有结点的路径长度，并且排好序。

因为一个结点最多在O(log n)个数组中出现，所以这些数组的总空间为O(n log n).另外每一个结点的数组显然可以由0（到自己的长度）+左儿子数组+右儿子数组 merge而来，那么每次合并使用归并排序，时间复杂度也是O(n log n)

做好这一步预处理后，我们怎么回答每一次询问呢？其实已经很简单了。对结点A，先统计A的子树上的路径，然后A的父节点上移动，统计A的兄弟子树上的路径；接着不断往上移动，直到根节点即可。因为树高为O(log n), 然后每一次统计时采用二分，所以每次查询的复杂度为O(log^2 n)

总复杂度为O(n*log n + m*log^2 n)
实现起来还是比较容易的。

## source code
```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
const int maxn = 1000010;
typedef long long ll;
int L[maxn];
vector<ll> dist[maxn], accDist[maxn];
int n, m;
void calDist(int i) {
    if (i <= 0 || i > n) return;
    if (2 * i <= n && 2 * i + 1 <= n) {
        calDist(2 * i);
        calDist(2 * i + 1);
        dist[i].push_back(0);
        for (int di = 0; di < dist[2 * i].size(); di++) {
            dist[i].push_back(dist[2 * i][di] + L[2 * i - 1]);
        }
        for (int di = 0; di < dist[2 * i + 1].size(); di++) {
            dist[i].push_back(dist[2 * i + 1][di] + L[2 * i]);
        }
        inplace_merge(dist[i].begin() + 1, dist[i].begin() + 1 + dist[2 * i].size(), dist[i].end());
    }
    else if (2 * i <= n && 2 * i + 1 > n) {
        calDist(2 * i);
        dist[i].push_back(0);
        for (int di = 0; di < dist[2 * i].size(); di++) {
            dist[i].push_back(dist[2 * i][di] + L[2 * i - 1]);
        }
    }
    else {
        dist[i].push_back(0);
    }
    ll acc_ = 0;
    for (int di = 0; di < dist[i].size(); di++){
        accDist[i].push_back((acc_ += dist[i][di]));
    }
}

ll forSubtree(int v, ll H) {
    if (H < 0 || v > n) return 0;
    int pos = upper_bound(dist[v].begin(), dist[v].end(), H) - dist[v].begin();
    return H * pos - accDist[v][pos - 1];
}

ll forPar(int v, int p, ll H) { // calculate happiness
    int v_ = (v == 2 * p ? 2 * p + 1 : 2 * p);
    return forSubtree(v_, H - L[v_ - 1]);
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> n >> m;
    for (int i = 1; i < n; i++) cin >> L[i];
    calDist(1);
    for (int mi = 0; mi < m; mi++) {
        int A, H;
        cin >> A >> H;
        ll ans = forSubtree(A, H);
        while (A > 1) {
            H -= L[A - 1];
            if (H <= 0) break;
            ans += H;
            ans += forPar(A, A / 2, H);
            A /= 2;
        }
        cout << ans << endl;
    }
}

```

