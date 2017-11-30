---
title: >-
  codeforces gym 101612 E [Equal Numbers] (ICPC 2017-2018 NEERC Northern
  Subregional Contest St Petersburg November 4 2017) 题解
comments: true
categories: ACM
tags:
  - math
abbrlink: 79e97c8b
date: 2017-11-29 22:30:09
---
## 题目描述
gym 101612 E题
链接： [gym](http://codeforces.com/gym/101612)

给定一个大小为n的正整数的数组，每次操作可以选取数组中一个数，将它乘上若干倍。求经过k次操作后，数组中最少有多少个不同的数？输出所有0<=k<=n的k的结果。

<!-- more -->


## 题目分析

样例输入是`n = 4, a[] = {1, 1, 2, 2, 3, 4}`

我们先将相同的数合并成一堆，比如这里`a[]`可以看作`{1(2), 2(2), 3(1), 4(1)}`

括号里的数表示这个数字的出现次数。

假定初始状态有`vn`堆数，这个样例里`vn = 4`

题目要求的就是经过若干次操作后，最少剩下多少堆。

不妨换个思路，我们来求把`vn`堆数合并成`vn - k`堆至少需要多少次操作。如果能求出这个结果，我们将数组逆一下，并且空白处的值用前面的值填充好就得到题目要求的答案。

下面思考把`vn`堆数合并成`vn - k`堆， 也就是减少k堆，至少需要多少次操作。

我们把原来的数分成两类，一类是它的倍数也存在于数组中，另一类是它没有一个倍数存在与数组中，分别记为A类，B类。

样例中A类为： `1(2), 2(2)`
样例中B类为： `3(1), 4(1)`

为了减少k堆，我们有两种决策方法：

1. 直接在A类中选取大小最小的k堆，将这些数全部提升为它们的倍数。
2. 在B类中选取大小最小的两堆，将这两堆数提升为所有数的最小公倍数，这时候减少了原有的2堆，但新增了1堆（所有数的最小公倍数），并且因为出现了所有数的最小公倍数，所以原有的所有数都变成A类，再按方法1选取(k-1)堆即可。

两种方法分别求出来比较一下，取最优就得到答案了。

源代码中用x表示第一种决策方式需要的操作数，y表示第二种决策方式需要的操作数。


## source_code
```c++
#include <cstdio>
#include <vector>
#include <cstring>
#include <algorithm>
using namespace std;
const int maxn = 1000060;

int a[maxn], cnt[maxn], ops[maxn], ans[maxn], bucket[maxn];
vector<int> v0, v1, v2;

void bucket_sort(vector<int> &v){
    memset(bucket, 0, sizeof(bucket));
    for (int i = 0; i < v.size(); i++) bucket[v[i]]++;
    v.clear();
    for (int i = 0; i < maxn; i++) 
    for (int j = 0; j < bucket[i]; j++) 
        v.push_back(i);
}


int main() {
    #ifndef LOCAL
    freopen("equal.in", "r", stdin);
    freopen("equal.out", "w", stdout);
    #endif
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", a + i);
        cnt[a[i]]++;
    }
    for (int i = 1; i < maxn; i++){
        if (cnt[i]){
            bool flag = false;
            for (int j = 2; i * j < maxn; j++) {
                if (cnt[i * j]) {
                    flag = true;
                    break;
                }
            }
            if (flag) v0.push_back(cnt[i]);
            else v1.push_back(cnt[i]);
        }  
    }
    int vn = v0.size() + v1.size();
    bucket_sort(v0);
    bucket_sort(v1);
    for (int i = 0; i < v0.size(); i++) v2.push_back(v0[i]);
    for (int i = 2; i < v1.size(); i++) v2.push_back(v1[i]);
    bucket_sort(v2);

    ops[vn] = 0;
    int x = 0, y = 0;
    if (v1.size() >= 2)   y+=v1[0] + v1[1]; else y = 2 * n;
    
    for (int k = 1; k < vn; k++) {
        if (k <= v0.size()) x += v0[k - 1]; else x = 2 * n;
        if (k > 1) y+=v2[k - 2];
       // printf("x = %d, y = %d, k = %d\n", x, y, k);
        ops[vn - k] = min(x, y);           
    }
    for (int i = 1; i <= vn; i++){
        ans[ops[i]] = i;
    }
    for (int i = 1; i <= n; i++){
        if (ans[i] == 0) ans[i] = ans[i-1];
    }
    for (int i = 0; i <= n; i++)
        printf("%d%c", ans[i], (i == n?10:32));

}
```

