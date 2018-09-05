---
title: CodeChef - SCIENCEF Science Fair 题解
comments: true
categories: ACM
tags:
  - DP
  - shortest path
  - graph
abbrlink: a5930d20
date: 2018-05-09 21:43:01
---

# 题目大意

有N个学生，住在不同的地方。现在学校要选学生去参加科技节活动。每个学生被选去的概率的百分数等于他们的百分制成绩。

大巴在S处出发，先去接送学校选去的学生，然后开到终点F。由于学生太过活跃，当大巴经过了他们家时，他们就算没被学校选中也会强行上车。每个学生有一个talkativeness，一次旅途的talkativeness等于所有上了车的学生的talkativeness的乘积。一次旅途的cost定义如下：

 cost(Trip) = Length(Trip) + (Talkativeness(Trip) % (1e9+7）)
 
 司机会选择一条使cost最小的路线行驶。问cost的期望。
 
<!-- more -->

# solution

扩展版旅行商问题。
关键是先重构图。压缩至只有S，F跟学生所在点。而这些点间的边权应该定义为原图中不经过别的学生的最短路长度。用$n^2$次dijkstra重构图完成后，就是一个旅行商问题了。之后状态压缩DP解决。

`dp1[x][i]`表示当前走到第i个点，访问过的点集为x的最小cost。
`dp2[x]`表示访问学生集合为x以及x的超集的最小cost。这就是最终答案了。

# source code

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll Mod = 1e9+7;
ll sum(ll a, ll b) {
    return (a + b) % Mod;
}
ll& Add(ll &a, ll b) {
    return a = sum(a, b);
}
ll product(ll a, ll b) {
    return a * b % Mod;
}
ll& Mul(ll &a, ll b) {
    return a = product(a, b);
}
const ll iv = 570000004LL;

ll talkative[1 << 17], ti[17], pi[20], mi[17];
int V, E, S, F, n;
const int N = 1020;
const ll INF = 253432145421354LL;
ll sp[20][20], d[N];
struct edge{
    int to;
    ll cost;
    edge(int to = 0, ll cost = 0):to(to), cost(cost){}
};
vector<edge> G[N];
int studentOnVertex[N];
void Mini(ll &a, ll b) {
    if (b < a) a = b;
}
typedef pair<ll, int> P;
typedef pair<ll, P> Tuple;
ll dp[1<<17][20], dp2[1<<17];

int main() {
    scanf("%d%d%d%d%d", &V, &E, &S, &F, &n);
    S--, F--;
    for (int i = 0; i < n; i++) {
        scanf("%lld%lld%lld", pi + i, ti + i, mi + i);
        pi[i]--;
    }
    for (int i = 0; i < E; i++) {
        int x,y,w;
        scanf("%d%d%d", &x, &y, &w);
        --x,--y;
        G[x].push_back(edge(y, w));
        G[y].push_back(edge(x, w));
    }
    talkative[0] = 1;
    for (int x = 0; x < (1 << n); x++) {
        for (int i = 0; i < n; i++) {
            if ((x & (1 << i)) == 0) {
                talkative[x|(1<<i)] = product(talkative[x], ti[i]);
            }
        }
    }
    memset(studentOnVertex, -1, sizeof studentOnVertex);
    for (int i = 0; i < n; i++) {
        studentOnVertex[pi[i]] = i;
    }
    pi[n] = S;
    pi[n + 1] = F;

    for (int s = 0; s <= n + 1; s++)
    for (int t = s + 1; t <= n + 1; t++) {
        fill(d, d + V, INF);
        d[pi[s]] = 0;
        priority_queue<P, vector<P>, greater<P> > que;
        que.push(P(0, pi[s]));
        while (!que.empty()) {
            P p = que.top(); que.pop();
            int v = p.second;
            if (p.first > d[v]) continue;
            for (auto e: G[v]) {
                int u = e.to;
                ll cost = e.cost;
                int stui = studentOnVertex[u];
                if (stui != -1 && stui != s && stui != t) continue;
                if (d[u] > d[v] + cost) {
                    d[u] = d[v] + cost;
                    que.push(P(d[u], u));
                }
            }
        }
        sp[s][t] = sp[t][s] = d[pi[t]];
    }

    for (int x = 0; x < (1 << n); x++)
    for (int i = 0; i <= n; i++)
        dp[x][i] = INF;
        
    dp[0][n] = 0;
    priority_queue<Tuple, vector<Tuple>, greater<Tuple> > que;
    que.push(Tuple(0, P(0,n)));

    while (!que.empty()) {
        Tuple tup = que.top(); que.pop();
        int x = tup.second.first, i = tup.second.second;
        if (tup.first > dp[x][i]) continue;
        for (int j = 0; j < n; j++) {
            if (j == i) continue;
            int nx = x | (1 << j);
            if (dp[nx][j] > dp[x][i] + sp[i][j]) {
                dp[nx][j] = dp[x][i] + sp[i][j];
                que.push(Tuple(dp[nx][j], P(nx,j)));
            }
        }
    } 
    for (int x = 1; x < (1 << n); x++) {
        dp2[x] = INF;
        for (int i = 0; i < n; i++) {
            if (x & (1 << i))  Mini(dp2[x], dp[x][i] + sp[i][n+1]);
        }
        dp2[x] += talkative[x];
    }

    for (int x = (1 << n) - 1; x > 0; x--) {
        for (int i = 0; i < n; i++)
            Mini(dp2[x], dp2[x|(1<<i)]);
    }
    ll ans = 0;
    for (int x = 1; x < (1 << n); x++) {
        ll temp = 1;
        for (int i = 0; i < n; i++) {
            if (x & (1 << i)) {
                Mul(temp, mi[i]);
            }
            else {
                Mul(temp, 100 - mi[i]);
            }
            Mul(temp, iv);
        }
        Mul(temp, dp2[x]);
        Add(ans, temp);
    }
    printf("%lld\n", ans);
}
```
