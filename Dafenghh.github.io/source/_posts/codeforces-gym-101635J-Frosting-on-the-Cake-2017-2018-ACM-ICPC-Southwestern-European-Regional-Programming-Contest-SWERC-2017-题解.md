---
title: >-
  codeforces gym 101635J [Frosting on the Cake] 2017-2018 ACM-ICPC, Southwestern
  European Regional Programming Contest (SWERC 2017) 题解
comments: true
categories: ACM
tags:
  - implementation
abbrlink: 8e378f9
date: 2018-01-26 14:32:45
---

# 题目大意
 将一块蛋糕，横切n-1刀，竖切n-1刀。使水平方向上，各块宽度分别为B1, B2...Bn；垂直方向上，各块宽度分别为A1, A2...An.
 
现在，按从左到右，从上到下的顺序，给每一小块依次循环染色“白 黄 粉”三种颜色。

求出每一种颜色的方块的总面积。

<!-- more -->

# 题目分析
其实就是一个简单的取余问题。


# source code
```c++
#include <bits/stdc++.h>
using namespace std;

int get(){
    char ch; int v,f=0;
    while (!isdigit(ch=getchar())) if (ch=='-') break;
    if (ch=='-') f=1;else v=ch-48;
    while (isdigit(ch=getchar())) v=v*10+ch-48;
    return f?-v:v;
} 

const int maxn = 100020;
typedef long long ll;
ll a[maxn], b[maxn];
ll am[3],ans[3];
int n;
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> n;
    for (int i = 0; i < n; i++) cin >> a[i], am[i%3] += a[i];
    for (int i = 0; i < n; i++) cin >> b[i];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < 3; j++)
            ans[j] += b[i] * (am[((ll)j-(ll)i*n-2+3*(ll)maxn*maxn)%3]);
    }
    cout << ans[0] <<" " << ans[1] << " " << ans[2] << endl;
    return 0;
}

```
