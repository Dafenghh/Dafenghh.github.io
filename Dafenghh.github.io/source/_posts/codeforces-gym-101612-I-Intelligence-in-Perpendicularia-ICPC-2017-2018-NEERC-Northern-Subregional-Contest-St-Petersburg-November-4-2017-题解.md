---
title: >-
  codeforces gym 101612 I [Intelligence in Perpendicularia] (ICPC 2017-2018
  NEERC Northern Subregional Contest St Petersburg November 4 2017) 题解
comments: true
categories: ACM
tags:
  - math
abbrlink: 7ec55acf
date: 2017-11-29 22:38:10
---

## 题目描述
gym 101612 I题
链接： [gym](http://codeforces.com/gym/101612)

给出一个多边形（只包含水平边和垂直边），求所有边中不能从外面看见的部分的长度。


<!-- more -->

## 题目分析
注意到能被外面看到的边经过平移后刚好能组成一个矩形，所以这题就简单了。先统计出周长，然后减去外接矩形的周长就得到答案了。


## source code
```c++
#include <fstream>
#include <algorithm>
using namespace std;


ifstream cin("intel.in");
ofstream cout("intel.out");


const int maxn = 2000;
typedef long long ll;
ll x[maxn], y[maxn];
int n;

ll abs_(ll x){
    return x > 0 ? x : -x;
}
ll getL(int i, int j) {
    if (x[i] == x[j]) return abs_(y[i] - y[j]);
    else return abs_(x[i] - x[j]);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> n;
    ll len = 0;
    ll minX, maxX, minY, maxY;
    for (int i = 0; i < n; i++) {
       cin >> x[i] >> y[i];
    }
    for (int i = 0; i < n; i++) {
       len += getL(i , (i + 1) % n);
       if (i == 0) minX = maxX = x[i], minY = maxY = y[i];
       else minX = min(minX, x[i]), maxX = max(maxX, x[i]),
            minY = min(minY, y[i]), maxY = max(maxY, y[i]);
    }
   // cout << len << endl;
    //cout << maxX << " " << minX << " " << maxY << " " << minY << endl;
    cout << len - 2 * (maxX - minX + maxY - minY) <<endl;
}

```