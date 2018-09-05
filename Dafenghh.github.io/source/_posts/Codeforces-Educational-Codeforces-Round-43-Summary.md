---
title: Codeforces Educational Codeforces Round 43 Summary
comments: true
categories: ACM
tags:
  - summary
  - codeforces
  - greedy
abbrlink: af1c4a82
date: 2018-05-01 09:57:28
---
# Summary

比赛中仅仅做出3道水题。

卡E。E一直想不出。曾有猜测“所有1st type spells全部用于一个creature”，但无法证明，也不敢下定论。没想到这就是正解。




--------


# 题解

## E. Well played!
### 题目大意
有n个物品，每个物品有两个值hp和dmg. 现有a个第一类道具和b个第二类道具。第一类道具可以使某个物品的hp翻倍，第二类道具可以将某个物品的hp赋值给dmg.

问使用了这些道具后，所有物品dmg和的最大值。

数据范围: $1\leq n\leq2\cdot10^5,0\leq b\leq2\cdot10^5,0\leq a\leq 20$


### solution
下面先证明，最优解一定是全部第一类道具用于同一个物品。分别用x和y表示物品的hp和dmg.

那么如果两个物品同时用了第一类道具，分别用了$a_1$和$a_2$个，它们的总dmg就是$d_1 = {x_1}2^{a_1}+{x_2}2^{a_2}$.

如果将第一类道具全部用于第1个物品，总dmg为$d_2={x_1}2^{a_1+a_2}+\max(x_2, y_2)$

假设d1更优，则有$${x_1}2^{a_1}+{x_2}2^{a_2} > {x_1}2^{a_1+a_2}+\max(x_2, y_2)$$

移项，有$${x_1}2^{a_1}(2^{a_2}-1) < {x_2}2^{a_2} - \max(x_2, y_2)$$

考虑到$\max(x_2, y_2) \geq x_2$,则有$${x_1}2^{a_1}(2^{a_2}-1) < {x_2}(2^{a_2}-1)$$

即$${x_1}2^{a_1}< {x_2}$$

同理，为了让$d_1$比第一类道具全部用于第2个物品的情况更优，有
$${x_2}2^{a_2}< {x_1}$$

两个不等式矛盾，证毕。

这样一来，就可以O(n)枚举每个物品使用第一类道具的情况。

### source code
```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 2e5+10;
struct P {
    ll h, d;
    bool used;
    P(ll h = 0, ll d = 0): h(h), d(d), used(false){}
    bool operator < (const P &b) {
        return h - d > b.h - b.d;
    }
}a[N];
void Maxi(ll &a, ll b) {
    if (b > a) a = b;
}
int main() {
    int n, A, b;
    scanf("%d%d%d", &n, &A, &b);
    ll k = 1;
    for (int i = 0; i < A; i++) k <<= 1;
    for (int i = 0; i < n; i++) {
        int h,d;
        scanf("%d%d", &h, &d);
        a[i] = P(h, d);
    }
    sort(a, a + n);
    ll tot = 0, cnt = 0;
    for (int i = 0; i < n; i++) {
        if (a[i].h > a[i].d && cnt < b) {
                ++cnt;
                tot += a[i].h;
                a[i].used = true;
        }
        else {
            tot += a[i].d;
        }
    }
    ll ans = tot;
    for (int i = 0; i < n; i++) {
        ll h = a[i].h * k;
        if (h <= a[i].d) continue;
        if (a[i].used) {
            Maxi(ans, tot - a[i].h + h);
        }
        else {
            ll temp = tot - a[i].d + h;
            if (cnt == b) {
                temp = temp - a[b - 1].h + a[b - 1].d;
            }
            Maxi(ans, temp);
        }
    }
    printf("%lld\n", ans);
}

```