---
title: 'POJ 3320 [Jessica''s Reading Problem] 题解'
comments: true
categories: ACM
tags:
  - 尺取法
  - hash
abbrlink: 6f199fb1
date: 2017-11-28 12:51:03
---
## 题目大意
给定一个大小为P的数组，取出一个子区间，要求这个子区间上的数能覆盖整个数组的数，求满足条件的子区间的最小长度。

<!-- more -->


## 题目分析
《挑战程序设计竞赛》P149例题，采用“尺取法”可以很快地做出这道题。

![挑战程序设计竞赛](/img/POJ3320_solution.jpg)

使用map来记录的时间复杂度为`O(P log P)`。

由于P最大为10e6，所以我提交的时候很担心超时。结果真的超时了，但并不是复杂度的锅，而是我用了cin cout，改成scanf printf就过了。

后来查了下，在windows下不开`ios::sync_with_stdio(false)`的优化时，cin比scanf慢差不多十倍;开了后，慢三倍。只有在linux下g++编译，并且写上这一句优化的情况下，cin才达到和scanf差不多的效率。

之后，将map改成hash，将时间复杂度降到线性，并且采用快速读入，这样才能47ms通过。

## source code
### solution 1 (454ms)
```c++
#include <cstdio>
#include <map>
#include <set>
#include <algorithm>
using namespace std;
const int maxn = 1000100;
int a[maxn];
int main() {

    int n;
    scanf("%d", &n);
    set<int> S;
    for (int i = 0; i < n; i++) {
        scanf("%d", a + i);
        S.insert(a[i]);
    }
    int sn = S.size();
    S.clear();
    map<int, int> M;
    int s = 0, t = 0, cnt = 0, ans = n;
    for (;;) {
        while (t < n && cnt < sn) {
            if (M[a[t]] == 0) cnt++;
            M[a[t]]++;
            t++;
        }
        if (cnt < sn) break;
        ans = min(ans, t - s);
        if (M[a[s]] == 1) cnt--;
        M[a[s]]--;
        s++;
    }
    printf("%d\n", ans);
}
```

### solution 2 (47ms)
```c++
#include <cstdio>
#include <map>
#include <cctype>
#include <set>
#include <algorithm>
using namespace std;
const int maxn = 1000100;
int a[maxn];
const int hash_n = 535442;
int M0[hash_n], M[hash_n];
inline int get()
{
    int k=0;
    char f=1;
    char c=getchar();
    for(;!isdigit(c);c=getchar() )
        if(c=='-')
            f=-1;
    for(;isdigit(c);c=getchar() )
        k=k*10+c-'0';
    return k*f;
}
int main() {
    int n;
    n = get();

    for (int i = 0; i < n; i++) {
        a[i] = get();
        a[i] %= hash_n;
        M0[a[i]]++;
    }
    int sn = 0;
    for (int i = 0; i < hash_n; i++) if (M0[i]) sn++;
    int s = 0, t = 0, cnt = 0, ans = n;
    for (;;) {
        while (t < n && cnt < sn) {
            if (M[a[t]] == 0) cnt++;
            M[a[t]]++;
            t++;
        }
        if (cnt < sn) break;
        ans = min(ans, t - s);
        if (M[a[s]] == 1) cnt--;
        M[a[s]]--;
        s++;
    }
    printf("%d\n", ans);
}
```