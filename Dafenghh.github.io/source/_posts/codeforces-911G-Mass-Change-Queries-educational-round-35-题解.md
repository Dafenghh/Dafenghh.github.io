---
title: 'codeforces 911G [Mass Change Queries] # educational round 35 题解'
comments: true
date: 2017-12-30 21:20:40
categories: ACM
tags:
  - data structures
  - segment tree
---

# 题目大意
给定一个数组a，长度为n，q次操作，每次操作给定四个整数，l,r,x,y表示将[l, r]区间中的值为x元素全部变成y，输出最终数组。

数据范围：`n, q <= 200000, a[i], x, y <= 100` 

# 题目分析

educational round的最后一题，本来以为非常难，看了[pannibal](http://codeforces.com/contest/911/submission/33740346) 的代码后，非常惊奇。这道题竟能如此简洁优美地解决。于是关掉网页，用自己的代码习惯重打一遍（当然大部分雷同哈哈，毕竟这个解法真的太简洁了）。

总结一下思路：

1. 本题的难点在于每次操作，需要更新[l, r]的整段区间。如果直接更新，复杂度将是`O(nq)` ，不可接受。

	我们采用类似于用前缀和快速查询区间和的思想，讲区间转化为两个端点处理，即在区间开始处l打上一个标记，表示从这里开始，x将被视作y ，再在区间结束后的r+1处打上标记，表示还原x的状态（将x重新视为x）。
	
	或者说，这个修改操作包含两个修改，(1) [l, +inf)区间的x变成y; (2) [r+1, +inf)区间的x变回x.
	
	

2. 另外，修改的顺序也是很重要的。我们将所有修改按照点的标记位置存在vector里。见main函数代码: 
```
vec[l].push_back(Change(t, x, y));
vec[r + 1].push_back(Change(t, x, x));
```
	其中t表示修改的顺序。这样一来，我们相当于将所有修改按点的位置重新排序。这么做有什么好处吗？其实，这就是这道题的关键转化步骤。

	作为一个经验尚浅的ACM选手，我对这题的直觉思路是依次考虑每次修改操作，寻找一种快速的更新区间方法。但注意到这道题只需要一个最终的结果，对中间结果并不关心。所以我们可以采用离线做法，不必完全跟从修改的次序来求解。

	那么我们采用怎样的求解次序呢？
	
	按数组下标依次求解最终结果。这也是很自然的思路，假如我们要求a[x]的最终结果，那么所有在x之后的点的更新对a[x]的值毫无影响。所以我们在输入完成后，依次拿出vec[i]的修改操作，来修改[i, +inf)这个区间. 这样在i后移的同时，我们就能依次得到a[i]的最终结果。
	
3. 下面讨论如果实现每次修改操作。

	比如现在我们要把x的值变成y，是不是要找出数组中所有的x，然后依次赋值成y呢？显然时间不允许这么做。
	
	我们只需要存一份转移表（transition table）即可。一开始所有数字无变化，对所有的i， 有`T[i] = i`。 当我们将x修改成y时，修改T[x] = y;
	
	如果这样一份转移表被构造出来，那么我们要求`a[i]`的最终结果就很简单了，那就是`T[a[i]]`
	
	但转移表既随时间（修改次序）变化，也随区间变化，即受两个维度影响，t维度（修改次序）和i维度（数组下标）。留意到，刚刚第2点说到，我们沿着i维度来提交修改。那么不妨，我们固定t维度，或称，保留下t维度的所有状态，即对每一个修改的时刻构造一张转移表。这道题最多200000次查询，也就是200000张转移表。
	
	比如我们在时刻t = 5和t = 6分别做了一次修改，得到两个转移表T_5, T_6, 很明显将T_5, T_6的转移表合并（合并的过程非常简单，相当于函数的组合）,就是这两次修改的最终效果。
	
	我们将所有转移表合并后得到的就是最终结果需要的转移表。而在i维度（数组下标）更新的过程中，我们需要快速地修改某些转移表，并将修改结果合并起来，得到a[i]的最终值。
	
	于是，线段树呼之欲出！线段树的每个叶子结点就代表一个时刻的转移表，它们的父结点就是合并后的转移表。对特定的x，所有修改合并操作都能在`O(log n)`时间内完成了。合并也只需要一条代码： `seg[t][i] = seg[chr][seg[chl][i]]`
	
	解法非常简洁优美！

# source code
```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = (1 << 18) - 1;
struct Change {
    int t, x, y;
    Change(int t, int x, int y):t(t),x(x),y(y){}
};

vector<Change> vec[maxn];
int a[maxn], seg[maxn * 2 + 20][101];

void printAll(int n) {
    for (int i = 1; i <= n; i++) printf("%d%c", seg[1][i], " \n"[i == n]);
}

void update(int t, int x, int y) {
    seg[t += maxn][x] = y;
    for (t >>= 1; t; t >>= 1) {
        int chl = t << 1, chr = chl | 1;
        for (int i = 1; i <= 100; i++) {
            seg[t][i] = seg[chr][seg[chl][i]];
        }
    }
}


void init() {
    for (int t = 1; t < maxn * 2 + 20; t++)
    for (int i = 1; i <= 100; i++)
        seg[t][i] = i;
}

inline int get() {
    int ans=0,flag=1;
    char c=getchar();
    while(c<'0' || c>'9'){
        if(c=='-') flag=-1;
        c=getchar();
    }
    while(c>='0' && c<='9'){
        ans=ans*10+(int)(c-'0');
        c=getchar(); 
    }
    return ans*flag;
}
int main() {
    int n = get(), q;
    for (int i = 1; i <= n; i++) a[i] = get();
    q = get();
    for (int t = 1; t <= q; t++) {
        int l = get(), r = get(), x = get(), y = get();
        if (x != y) {
            vec[l].push_back(Change(t, x, y));
            vec[r + 1].push_back(Change(t, x, x));
        }
    }
    init();

    for (int i = 1; i <= n; i++) {
        for (auto cg: vec[i]) update(cg.t, cg.x, cg.y);
        printf("%d%c", seg[1][a[i]], " \n"[i == n]);
    }

}

```
