---
title: 'POJ 2104 [K-th Number] 题解'
comments: true
categories: ACM
tags:
  - segment tree
  - sqrt decomposition
  - 主席树
  - data structures
abbrlink: 76b5095a
date: 2018-01-11 09:51:08
---
# 题目大意
给定一个长度为n的数组，m次查询。每次查询给出三个数（i, j, k），表示要求原数组的区间[i, j]升序排列中的第k个数。


<!-- more -->

# 题目分析
求区间第k小的经典题目！来源于《挑战程序设计竞赛》P185例题。然后发现，这道题在4个月之前，我学习主席树的时候就做过一次。

书上提供了平方分割和归并树的两种做法，下面我也将重现下这两种解法以及主席树的解法，并在最后对这三种优雅的解法作下对比。

## solution 1 (分块)

假如题目提供了一个函数cnt(i, j, x)，返回原数组区间[i, j]中小于x的数的个数。那么我们要求的区间[i,j]第k小的数，也就是要求满足cnt(i, j, x) < k的最大的x。

于是，对x进行二分搜索，即可得到答案。

那么，我们怎么样才能自己写出一个类似于cnt(i, j, x)的函数呢？

假如数组区间[i, j]是有序的，那么我们很容易用lower_bound写出这个函数。

虽然原数组不是有序的，但这么想能够给我们带来灵感。

我们不妨把原数组分成若干块区间，然后对每一块区间进行排序。这就是分块算法。这样，数组在每一块区间内都是有序的。

于是对于要查询的区间[i, j]所包含的那些完整的区间块，我们采用lower_bound算出cnt值。

而区间[i, j]两端有一些元素是不在一个完整区间块的，对这些元素逐一检查即可。（反正这部分元素的个数不超过两倍块的长度，所以数量较少，逐一检查OK）


## solution 2 (归并树)
除了使用分块算法算出函数cnt(i, j, x)的值外，我们还可以采用归并树的做法。

这里的归并树是一种特别的线段树，它完整记录下了归并排序的每一步结果。

也就是，归并树的每一个结点，维护的是一个vector，这个vector就保存着结点对应区间排序后的结果。

于是，建树的过程也就是归并排序的过程，只不过每一步merge的时候，要把中间的结果保存在线段树的结点的vector里。

所以我们计算cnt(i, j, x)的值的时候，用线段树的思想求解即可。

即，若查询区间与当前结点对应区间无交集，返回0；查询区间完整包含当前结点对应区间在内，则对当前结点的vector采用lower_bound返回结果；否则对线段树左右儿子递归查询，求和即可。


## solution 3 (主席树)
前面两种做法都是采用使数组部分有序后统计cnt值的思想。Solution1 中的部分有序指块数组的部分有序，Solution2中部分有序指线段树维护区间的部分有序。

而主席树的做法采用完全不一样的思路。

主席树也是一种特殊的线段树。它不是像Solution2或者往常RMQ问题一样，维护原数组的区间；而是像维护值域的区间。

就是说，假如这个特殊的线段树的一个结点，维护的区间是[l, r), 记录值为dat，那么dat的意义是原数组中有多少个值位于[l, r)的范围内。

现在考虑，假设我们已经对要查询的区间[i, j]构造了这样一棵线段树，要查询第k小的值，怎么找？

其实很简单，从根结点找起，其实根结点对应的区间就是整个值域。考察左儿子的dat值，如果它大于等于k，也就是说有大于等于k个数位于左边的值域，于是我们就对左儿子进行递归查找；否则查找右儿子。直到查找的区间长度为1，那么这个就是答案。

那么，假如我们对所有O(n^2)个区间都建这样一棵关于值分布的线段树，我们就能对任意区间，查询到第k小的答案了。

显然，O(n^2)棵线段树是不现实的，根本没有这么多空间。

用一下前缀和的思想，考虑原数组区间[i, j]对应的线段树，其实它可以由[1, j]和[1, i - 1]两棵线段树的值作差而来。（这里的“作差”就是对线段树每一个结点的dat值求一次差）

所以我们实际上，只需要n棵线段树即可，n指值域长度。

但是， O(n)棵线段树仍然不现实，没有这么多空间。

主席树的巧妙之处就在这里。

考察第i棵线段树和第i+1棵线段树的区别，也就是原数组区间[1, i]和[1, i + 1]分别对应的线段树。我们发现，后一棵线段树，比前一棵线段树，仅仅多更新了原数组中的一个数，即a[i+1].

这样一来，两棵线段树仅仅只有从根到a[i+1]对应的叶这一条链是不同了（增加了1），其他结点完全相同。于是我们，只给这更新的一条链创建新结点可以，其他结点沿用旧的线段树的结点即可。

那么，更新的线段树高度为O(log n)，所以会有O(log n)个结点更新，也就是，这棵线段树实际只占用O(log n)的内存空间，但在逻辑上，它依然是棵完整的线段树。

另外，对于第零棵线段树，即原数组区间[1, 0]对应的线段树，所有结点dat值都为0，所以这整棵线段树用一个零结点来存就可以。

这样一来，我们就能够实现，只用了O(nlogn)的内存空间，存放下了n + 1棵线段树（包括第零棵）的信息，因为很多结点都被多棵线段树共享了嘛。


P.S.其实这种做法与第二种归并树的做法，有异曲同工之妙。归并树是把归并排序的中间结果全部记录下来。而主席树，实际上也是，把更新时的中间结果全部记录下来。对这样一棵表示值分布的线段树，我们依次从左往右拿原数组的值去更新线段树，每一次更新会修改O(log n)个结点，而主席树没有直接修改原树上的O(log n)个结点，而是新建了O(log n)个结点。这样，线段树在动态更新的过程中，每一个历史版本都被完整记录。

# source code
## solution 1 (分块)
```c++
#include <cstdio>
#include <algorithm>
#include <vector>
#include<cctype>
using namespace std;
const int maxn = 100002;
const int B = 1000;
int bucket[maxn / B][B];
int a[maxn],nums[maxn];

inline int read()
{
    int X=0,w=0; char ch=0;
    while(!isdigit(ch)) {w|=ch=='-';ch=getchar();}
    while(isdigit(ch)) X=(X<<3)+(X<<1)+(ch^48),ch=getchar();
    return w?-X:X;
}
int main() {
    int n = read(), m = read();
    
    for (int i = 0; i < n; i++) {
        a[i] = read();
        bucket[i/B][i%B] = a[i];
        nums[i] = a[i];
    }
    sort(nums, nums + n);
    for (int i = 0; i < n / B; i++) sort(bucket[i], bucket[i] + B);
    
    while (m--) {
        int l = read(), r = read(), k = read(); 
        int ul = 0, ur = n;
        while (ul + 1 < ur) {
            int mid = (ul + ur) >> 1;
            int cnt = 0, tl = l - 1, tr = r; // the number of items less than nums[mid]
            while (tl < tr && tl % B) cnt += (a[tl++] < nums[mid]); 
            while (tl < tr && tr % B) cnt += (a[--tr] < nums[mid]);
            while (tl < tr) {
                int bi = tl / B;
                cnt += lower_bound(bucket[bi], bucket[bi] + B, nums[mid]) - bucket[bi];
                tl += B;
            }
            if (cnt >= k) ur = mid;
            else ul = mid;
        }
        printf("%d\n", nums[ul]);
    }
    
}
```

## solution 2 (归并树)
```c++
#include <algorithm>
#include <cctype>
#include <cstdio>
#include <iostream>
#include <vector>
using namespace std;
inline int read()
{
    int X=0,w=0; char ch=0;
    while(!isdigit(ch)) {w|=ch=='-';ch=getchar();}
    while(isdigit(ch)) X=(X<<3)+(X<<1)+(ch^48),ch=getchar();
    return w?-X:X;
}

const int maxn = 100010;
int a[maxn],n,m,nums[maxn];
vector<int> vec[maxn * 20];

void build(int v = 1, int l = 0, int r = n) {
    if (l >= r) return;
    if (l + 1 == r) {
        vec[v].push_back(a[l]);
    }
    else {
        int mid = (l + r) >> 1, chl = v * 2, chr = v * 2 + 1;
        build(chl, l, mid);
        build(chr, mid, r);
        vec[v].resize(vec[chl].size() + vec[chr].size());
        merge(vec[chl].begin(), vec[chl].end(), vec[chr].begin(), vec[chr].end(), vec[v].begin());
    }
}

int query(int L, int R, int x, int v = 1, int l = 0, int r = n) {

    if (l >= r || r <= L || R <= l) return 0;
    if (L <= l && r <= R) {
        int res = lower_bound(vec[v].begin(), vec[v].end(), x) - vec[v].begin();
        return res;
    }
    int mid = (l + r) >> 1, chl = v * 2, chr = v * 2 + 1;
    int res= query(L, R, x, chl, l, mid) + query(L, R, x, chr, mid, r);
    return res;
}

int main() {
    n = read(),m = read();
    for (int i = 0; i < n; i++) {
        nums[i] = a[i] = read();
    }
    sort(nums, nums + n);
    build();
    while (m--) {
        int l = read(), r = read(), k = read();
        l--;
        int ul = 0, ur = n;
        while (ul + 1 < ur) {
            int mid = (ul + ur) >> 1;
            if (query(l, r, nums[mid]) >= k) ur = mid;
            else ul = mid;
        }
        printf("%d\n", nums[ul]);
    }
}
```

## solution 3 (主席树)
```c++
#include <cstdio>
#include <cctype>
#include <algorithm>
using namespace std;
const int maxn = 100010;
inline int read()
{
    int X=0,w=0; char ch=0;
    while(!isdigit(ch)) {w|=ch=='-';ch=getchar();}
    while(isdigit(ch)) X=(X<<3)+(X<<1)+(ch^48),ch=getchar();
    return w?-X:X;
}
struct node{
    int l, r, dat;
    node(int l = 0, int r = 0, int dat = 0):l(l),r(r),dat(dat){}
}T[maxn * 20];
int rt[maxn], a[maxn], b[maxn], sz = 0;

void update(int &o, int x, int last, int l, int r) { // [l, r)
    o = ++sz;
    T[o] = T[last];
    T[o].dat++;
    if (l + 1 >= r) return;
    int mid = (l + r) >> 1;
    if (x < mid) update(T[o].l, x, T[last].l, l, mid);
    else update(T[o].r, x, T[last].r, mid, r);
}

int query(int t1, int t2, int l, int r, int k) { // [l, r)
    if (l + 1 >= r) return l;
    int cnt = T[T[t2].l].dat - T[T[t1].l].dat, mid = (l + r) >> 1;
    if (cnt >= k) return query(T[t1].l, T[t2].l, l, mid, k);
    return query(T[t1].r, T[t2].r, mid, r, k - cnt);
}

int main() {
    fill(T, T + maxn * 20, node());
    fill(rt, rt + maxn, 0);
    int N = read(), Q = read();
    for (int i = 0; i < N; i++) b[i] = a[i] = read();
    sort(b, b + N);
    int n = unique(b, b + N) - b;
    for (int i = 0; i < N; i++) {
        a[i] = lower_bound(b, b + n, a[i]) - b;
        update(rt[i + 1], a[i], rt[i], 0, n);
    }
    while (Q--) {
        int l = read(), r = read(), k = read();
        printf("%d\n", b[query(rt[l - 1], rt[r], 0, n, k)]);
    }
}
```

# 总结
|Solution           |Time(ms)| Memory(MB)  | Code Length |
|:----------------: |:------:|:-----------:|:-----------:|
|分块 |11516   |1.5          |   1337      |
|归并树|6344    |37.1         |   1662      |
|主席树              |1735    |25           |1469|

最后送上三种解法的耗时，内存占用和代码长度的直观对比。

分块的实现最简单，代码最短，空间开销也最小，但非常慢，濒临超时。实际上，如果不加读入优化的话，那么分块就超时了。可见，非常凶险。

完美复现归并排序的归并树显然内存消耗是最高的，因为采用的是和分块一样的思路，也要有两层的二分查找，所以即使使用了线段树，但总耗时也只是节省了一半不到。

最后一种，主席树的做法，从原理上来讲，最复杂，但同时也最优美。从表中可以看出，时间比归并树快很多，空间开销和代码长度都最小，所以毫无疑问是本题的最佳解法。

P.S. 有同学可以提供比主席树更好的做法吗？感激不禁。（发现自己能在不同解法的对比中学到更多）
