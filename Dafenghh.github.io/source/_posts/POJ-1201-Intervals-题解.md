---
title: 'POJ 1201 [Intervals] 题解'
comments: true
date: 2018-01-17 11:18:14
categories: ACM
tags:
  - greedy
  - BIT
  - data structures
  - graphs
  - 差分约束
---
# 题目大意
给定n个区间，现在要从每个区间$[a_i, b_i]$中取出$c_i$个数，所有被取出的数组成一个集合。求这个集合的最小size.

<!-- more -->

# 题目分析
这种区间覆盖问题，首先考虑贪心法。如果将所有顶点按右端点排序后，依次取数。对每一个区间，尽量取靠右边的数。这是一个挺好的贪心策略，容易证明其正确性。


那么，对每个区间，需要做的事情有两步：

1. （统计）统计这个区间上已经被取过的数的数量，如果已满足要求，则OK；否则进行第二步。
2. （取数）从右往左依次取新数，直到满足要求。

朴素做法，统计这一步就需要O(n)复杂度，总计O(n^2), 显然不可行。

我们可以采用BIT，把统计这一步优化到O(log n）。

但第二步，取数的复杂度呢？

直觉来看，最坏情况下，每次取数要O(n)的时间，那么总体是O（n^2）复杂度。

但是，考虑到区间长度有限，也是O(n)的级别，所以实际上达到最坏情况的区间很少；换句话说，需要频繁取数的区间是很少的。（如果一个区间取出了很多数，那么相应的，它之后的重叠区间需要取数的区间个数就会少一些。）

于是，第二步用朴素算法即可。虽然没能估计确切的复杂度，但提交后跑起来很快，94ms就过了。


第二种做法，转化成差分约束问题。

如果用d[i]表示做法一中，BIT的前缀和，那么条件(区间[a,b]中有c个数被取出来)
可以表示成不等式`d[b]-d[a-1]>=c`

有了这个不等关系，就可以很方便的转成差分约束问题了，另外还要加上初始约束：`d[i]<=d[i+1]<=d[i]+1`

很好理解。

然后求解最短路，即可得到答案。

这道题我们求的是d[Max] - d[Min]的最小值，但是最短路求出来后对应的是一个最大值。

于是我们可以加个负号，求出d[Min] - d[Max]的最大值x，那么-x就是答案。如果一开始将d[Max]赋值为0，那么最后答案就是-d[Min].

采用经队列优化的Bellmen Ford算法，最坏情况下复杂度仍然为O(VE).但考虑到这题中的图对应了一个规则良好的差分约束系统，很难出现一个极端不均匀的图。所以这种做法耗时仅200ms。

P.S. POJ没有开O2优化，所以用vector表示邻接表，再一次跪了……


# source code
## solution 1 (贪心+BIT)
```c++
#include <cstdio>
#include <algorithm>
#include <cctype>
using namespace std;
inline int read()
{
    int X=0,w=0; char ch=0;
    while(!isdigit(ch)) {w|=ch=='-';ch=getchar();}
    while(isdigit(ch)) X=(X<<3)+(X<<1)+(ch^48),ch=getchar();
    return w?-X:X;
}
const int maxn = 50050;
int bit[maxn];
int lowbit(int i) {
    return i & -i;
}
int sum(int i) {
    int s = 0;
    for (; i; i -= lowbit(i)) s+= bit[i];
    return s;
}

void add(int i, int x) {
    for (; i < maxn; i += lowbit(i)) bit[i] += x;
}

struct node {
    int l, r, c;
    bool operator < (const node &b)const {
        return r < b.r;
    }  
}a[maxn];
bool used[maxn];


int main() {
    int n = read();
    for (int i = 0; i < n; i++) a[i].l = read() + 1, a[i].r = read() + 1, a[i].c = read();
    sort(a, a + n);
    for (int i = 0; i < n; i++) {
        int need = a[i].c - (sum(a[i].r) - sum(a[i].l - 1));
        for (int x = a[i].r; x >= a[i].l && need > 0; x--) {
            if (!used[x]) {
                used[x] = true;
                add(x, 1);
                need--;
            }
        }
    }
    printf("%d\n", sum(maxn - 1));
}
```
## solution 2 (差分约束)
```c++
#include <cstdio>
#include <algorithm>
#include <queue>
#include <cctype>
#include <cstring>

using namespace std;
inline int read()
{
    int X=0,w=0; char ch=0;
    while(!isdigit(ch)) {w|=ch=='-';ch=getchar();}
    while(isdigit(ch)) X=(X<<3)+(X<<1)+(ch^48),ch=getchar();
    return w?-X:X;
}
const int maxn = 50050, INF = 12345634;
struct edge{
    int to, cost, next;
    edge(int to = 0, int cost = 0, int next = 0):to(to), cost(cost), next(next){}
}a[4 * maxn];
int head[maxn], ei = 0;
void add_edge(int u, int v, int c) {
    a[ei] = edge(v, c, head[u]);
    head[u] = ei++;
}

bool used[maxn];
int d[maxn];

void Mini(int &a, int b) {
    if (b < a) a = b;
}

void Maxi(int &a, int b) {
    if (b > a) a = b;
}

int main() {
    memset(head, -1, sizeof(head));
    int n_ = read();
    int L = INF, R = -INF; // [L, R]
    for (int i = 0; i < n_; i++) {
        int a = read() + 1, b = read() + 1, c = read();
        add_edge(b, a - 1, -c);
        Mini(L, a - 1);
        Maxi(R, b);
    }
    for (int i = L; i < R; i++) add_edge(i, i + 1, 1), add_edge(i + 1, i, 0);
    fill(d, d + R + 10, INF);
    d[R] = 0;
    queue<int> que;
    que.push(R);
    used[R] = true;
    while (!que.empty()){
        int v = que.front();
        que.pop();
        used[v] = false;

        for (int i = head[v]; i != -1; i = a[i].next) {
            int to = a[i].to, cost = a[i].cost;
            if (d[v] + cost < d[to]) {
                d[to] = d[v] + cost;
                if (!used[to]) que.push(to), used[to] = true;
            }
        }
    }
    printf("%d\n", -d[L]);
}
```