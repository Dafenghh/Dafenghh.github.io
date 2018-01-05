---
title: 'POJ 3109 [Inner Vertices] 题解'
comments: true
date: 2018-01-05 16:17:54
categories: ACM
tags:
- BIT
- scanline
---
# 题目大意
一个无限大的棋盘，给出N个点的坐标，初始时这些点上都放置着黑棋。其他所有点放置着白棋。若一个白棋的上下左右方向上都有黑棋，那么它会被替换成黑棋。求最终黑棋的数量。


<!-- more -->

# 题目分析
与cf 911G如出一辙的扫描线算法。

将所有点按y坐标排序后（y相同时按x排序），我们可以依次统计，相邻的两个y坐标相同的点之间的白点对答案的贡献。

假设这两个点为(x1, y0) (x2, y0) 那么我们要算的就是$\{(x, y) | x \in [x1 + 1, x2 - 1], y = y0\}$的范围内有多少个
白棋变成了黑棋。

对于此范围中的一个点(xx, yy)，这一个点变成黑棋的充要条件是，(xx, yy)的上方和下方还有别的黑棋。

假设在原黑棋的点集合中，当x = xx时，y的最大、最小值分别为Max, Min。那么上面这个充要条件就可以表示为Min < y < Max.

我们很容易想到，当访问到y = yy的最低点也即(xx, Min)时，给x加上标记（表示此条线x == xx上的点将可能变黑）, 当访问到最高点即(xx, Max)时，给x消除标记（表示此条线x == xx上的点将不再会变黑）。

用BIT快速统计出区间[x1 + 1, x2 - 1]上会变黑的点的数量, 本题完成。

P.S. 这题做得也挺尴尬的，一开始写的扫描线算法还要统计出对每个x、y对应的y、x的极值，然后用很累赘的方法打标记，用了很多次vector，导致TLE。后来把vector全部去掉，全部换成数组才过。

感谢此篇博文[by lolicon480](http://blog.csdn.net/lolicon480/article/details/44183397) 提供的思路！
# source code
```c++
#include <cstdio>
#include <vector>
#include <utility>
#include <map>
#include <set>
#include <algorithm>
#include <ctime>

using namespace std;
typedef long long ll;
const ll maxn = 200010;
ll cnt = 200020;
ll bit[maxn];

void Maxi(int &a, int b) {
    if (b > a) a = b;
}

ll lowbit(ll x) {
    return x & -x;
}
ll sum(ll i) {
    ll res = 0;
    for (; i > 0; i -= lowbit(i)) res += bit[i];
    return res;
}
void add(ll i, ll x) {
    for (; i <= cnt; i += lowbit(i)) bit[i] += x;
}

struct P {
    int x, y;
    P(int x = 0, int y = 0):x(x), y(y) {}
}a[maxn];

bool cmpx(const P&a, const P&b){
    if (a.x != b.x) return a.x < b.x;
    return a.y < b.y;
}
bool cmpy(const P&a, const P&b){
    if (a.y != b.y) return a.y < b.y;
    return a.x < b.x;
}

int mx[maxn];
bool sc[maxn]; // scope
int main() {
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) scanf("%d%d", &a[i].x, &a[i].y);
    sort(a, a + n, cmpx);
    int val = 1;
    for (int i = 0; i < n; i++) {
        int temp = a[i].x;
        a[i].x = val;
        if (temp != a[i + 1].x) val++;
    }
    sort(a, a + n, cmpy);
    val = 1;
    for (int i = 0; i < n; i++) {
        int temp = a[i].y;
        a[i].y = val;
        if (temp != a[i + 1].y) val++;
    }
    for (int i = 0; i < n; i++)
        Maxi(mx[a[i].x], a[i].y);
    ll ans = 0;
    for (int i = 0; i < n - 1; i++) {
        int x = a[i].x, y = a[i].y, nx = a[i + 1].x, ny = a[i + 1].y;
        if (!sc[x] && y < mx[x]) {
            sc[x] = true;
            add(x, 1);
        }
        if (y == ny && nx > x + 1) {
            ans += sum(nx - 1) - sum(x);
        }
        if (sc[x] && y == mx[x]) {
            sc[x] = false;
            add(x, -1);
        }
    }
    ans += n;
    printf("%lld\n", ans);
}

```
