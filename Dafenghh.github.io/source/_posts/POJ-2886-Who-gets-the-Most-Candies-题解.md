---
title: 'POJ 2886 [Who gets the Most Candies] 题解'
comments: true
categories: ACM
tags:
  - BIT
  - data structures
abbrlink: 63c20c7e
date: 2018-01-10 14:46:27
---
# 题目大意
N个孩子围一圈玩游戏， 按顺时针从1到N编号，每个孩子手上有张牌，牌上有个非零数字。给定数字K，一开始第K个孩子走出圈。每个出圈的孩子的牌上的数字将决定下一个出圈孩子是谁。若此时出圈孩子的牌的数字为x，正则往顺时针方向数到第x个孩子，负则逆时针方向数到第|x|个孩子，即为下一个要走出圈的孩子。

第i个出圈的孩子将获得F[i]个糖果。F[i]定义为正整数i的因数个数。输出获得糖果最多的孩子的名字和糖果数。若有多个答案，则输取出圈最早的孩子作为答案。

<!-- more -->

# 题目分析
采用取模的方法很容易算出当前需要出圈的是序列中第几个孩子。但这题难在，如果动态、快速地将一个序列中的一项删除呢？

不妨不存储实际的序列，而只是用一个BIT（树状数组）来表示每个孩子是否在场。

初始时，1-N的每个点的值都为1，表示每一个孩子都在场。若有一个孩子离场，则将它赋值为0即可。

很容易想到，若编号为i的孩子在场，那么前缀和sum(i)就表示他现在在队伍中的实际位置。

所以采用二分法，就可以快速确定队伍中排在第k位的孩子是谁。

时间复杂度为$O(n\log^2 n)$ n最大为500000，担心超时。但BIT采用lowbit来算的话，时间节省一半，即带上一个1/2的系数。

那么$0.5\times 500000 \times log_2^2 500000 = 8.96 \times 10^7 $

所以时间复杂度在可接受的范围内。

另外，算F[i]时直接用素数筛的方法做一遍预处理即可， O(n log n)的复杂度。


# source code

```c++
#include <cstdio>
#include <queue>
#include <ctime>
#include <cstring>
#include <algorithm>
using namespace std;
const int maxn = 500010;
int F[maxn];
int n, k;
void initF(){
    for (int i = 1; i < maxn; i++) {
        for (int j = 1; i * j < maxn; j++) F[i * j]++;
    }
}

int bit[maxn];
int lowbit(int x) {
    return x & -x;
}
int sum(int i) {
    int s = 0;
    for (; i; i -= lowbit(i)) s += bit[i];
    return s;
}

void add(int i, int x) {
    for (; i <= n; i += lowbit(i)) bit[i] += x;
}

struct kid {
    int p, id;
    kid(int p = 0, int id = 0):p(p), id(id) {}
    bool operator < (const kid &b)const {
        if (F[p] != F[b.p]) return F[p] < F[b.p];
        return p > b.p;
    }
};
priority_queue<kid> que;
int cards[maxn];
char names[maxn][12];


typedef long long ll;
ll tr(ll x, ll n) {
    x += n * 100000000LL;
    return x % n;
}

int getP(int k) {
    int l = 0, r = n;
    while (l + 1 < r) {
        int mid = (l + r) >> 1;
        if (sum(mid) < k) l = mid; else r = mid;
    }
    return r;
}


int solve(int k, int step, int n) {
    int ni = getP(k);
    add(ni, -1);
    que.push(kid(step, ni));
    if (n == 0) return 0;
    k += -1 + cards[ni];
    if (cards[ni] > 0) k--;
    if (k < 0) k = (int)tr(k, n);
    k %= n;
    k++;
    return k;
}
void Clear(priority_queue<kid> &que) {
    priority_queue<kid> q1;
    swap(q1, que);
}

int main() {
    initF();

    while (scanf("%d%d", &n, &k) != EOF) {
        for (int i = 1; i <= n; i++)   scanf("%s%d", names[i], cards + i);
        Clear(que);
        memset(bit, 0, sizeof(bit));
       for (int i = 1; i <= n; i++) add(i, 1);
        for (int i = 1; i <= n; i++) {
            k = solve(k, i, n - i);
        }
        kid ans = que.top();
        printf("%s %d\n", names[ans.id], F[ans.p]);
    }
}


```
