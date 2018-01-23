---
title: >-
  NWERC 2017 A [Ascending Photo] (The 2017 Northwestern Europe Regional Contest)
  题解
comments: true
tags:
  - dp
abbrlink: 7c1f2f41
date: 2018-01-22 23:08:38
categories:
---
# 题目大意

[题目链接](https://open.kattis.com/contests/nwerc17open/problems/ascendingphoto)

给定一个序列，将它切割成若干段，使得对每一段进行移动之后可以重排成非严格升序。求最少的切割数。


<!-- more -->


# 题目分析
（集训的时候就差DP这步没想出来。）

首先先将相邻的相同元素合并，并进行离散化（不影响答案）。

然后现在将数组的每一个元素都切开（即切n-1下），比如1|2|3|0|4

于是问题可以转化为，这n-1块“挡板”最多有多少块可以去掉，如果为s，那么最终答案就是n-1-s.

在上面的例子中，唯一能够去掉的挡板是(2,3), 即变成1|2 3|0|4

很显然，挡板能够去掉首先要满足，左右两个元素相差1.

另外，假如有多块(2,3)挡板，比如对于1|2|3|4|2|3

(2,3)的挡板只能去掉其中一块（不然装不回来）。所以，我们可以先将所有值出现的位置保存到vector中，然后从小到大考虑值，每两个相邻的值应该去掉哪一块挡板（有可能去不掉）。

另外，如果去掉前面的(2,3)挡板，这时候会发现，紧接着后面的(3,4)挡板就去不了了。

否则会变成：1|2 3 4|2|3

可以看到最右边的3落下了。


所以可以看到，去掉一个挡板有可能会产生一个冲突位置。具体地说，如果存在连续序列(a,a+1,a+2), 并且a+1在数组中不唯一，那么去掉(a,a+1)挡板将会导致(a+1,a+2）的挡板无法去除。

用`dp[i]`表示去除值(0,1)到值(i,i+1)的所有可去除的挡板数量。

注意到，`dp[i]`的值与每一步去除哪一块可选挡板有关，所以要加多一维。`dp[i][j]`的j表示，去除值(i,i+1)的挡板时，考虑的是位置(j,j+1)

即首先i和j有条件，`h[j] = i, h[j+1] = i+1`

`dp[i][j] = max(dp[i-1][j'] + （j是不是j'产生的冲突位置?0:1）)`


“j是不是j'产生的冲突位置” 等价于`j = j' + 1 && h[j]不唯一 `

从这个条件可以看出，对于一个j'，最多产生一个冲突位置，j'+1(当h[j'+1]唯一就不是冲突位置）。

对于一个j，只可能是一个位置j' = j - 1的冲突位置。（条件*）

如果我们把dp[i]看成一张表，那么最后我们求的就是dp[n]的最大值。

观察转移方程，从dp[i]向dp[i+1]转移的时候，求max的是dp[i]这张表的所有元素，但其中有些元素加了1（条件j不是j'的冲突位置）。

所以，我们只需保存这张表最大的那些值就行了。比如某个dp[i]={1,2,3,4,5,5,5}, 我们只需要保存{5,5,5}就行，因为前面的数，比如4，加上1也才是5，并不会更优。

dp[i+1]的值有可能是5，也有可能是6，要看三个5之中，有没有一个5对应的冲突位置不是j，这个5就能加上1，变成6.

回顾条件*，当我们保存了两个dp值最大的j'对应的冲突位置时，那么求 `dp[i][j]` 就能一定找到一个不冲突的位置，然后累计加上1.

所以，每一步dp求出的表中，只需要保存最大两个值以及它们对应的冲突位置即可，下面代码，用best[0]和best[1]保存最大两个值以及对应的冲突位置。

# source code
```c++
#include <bits/stdc++.h>
using namespace std;
int get(){
	char ch; int v=0,f=0;
	while (!isdigit(ch=getchar())) if (ch=='-') break;
	if (ch=='-') f=1;else v=ch-48;
	while (isdigit(ch=getchar())) v=v*10+ch-48;
	return f?-v:v;
} 

typedef pair<int, int> P;
const int maxn = 1000020;
int b[maxn];
int main() {
    vector<int> H;
    int n = get();
    for (int i = 0; i < n; i++) {
        int x = get();
        if (H.empty() || H.back() != x) H.push_back(x);
    }
    n = (int)H.size();
    for (int i = 0; i < n; i++) b[i] = H[i];
    sort(b, b + n);
    int sz = unique(b, b + n) - b;
    vector<vector<int> > posi(sz);
    for (int i = 0; i < n; i++) H[i] = lower_bound(b, b + sz, H[i]) - b, posi[H[i]].push_back(i);
    P best[2] = {P(0, n), P(0, n)};
    for (int h = 0; h < sz - 1; h++) {
        P nbest[2] = {best[0], best[1]};
        for (int i = 0; i < posi[h].size(); i++) {
            int p = posi[h][i];
            if (p == n - 1 || H[p] + 1 != H[p + 1]) continue;
            P s(0, n);
            if (p != best[0].second) s = best[0];
            else s = best[1];
            s.first++, s.second = p + 1;
            if (posi[h + 1].size() == 1) s.second = n;
            if (s > nbest[0]) nbest[1] = nbest[0], nbest[0] = s;
            else if (s > nbest[1]) nbest[1] = s;
        }

        best[0] = nbest[0], best[1] = nbest[1];
    }
    printf("%d\n", n - 1 - best[0].first);    
}
```