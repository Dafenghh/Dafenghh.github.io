---
title: 'POJ 1064 [Cable master] 题解'
comments: true
categories: ACM
tags:
  - binary search
abbrlink: 52b6f961
date: 2017-11-27 15:13:40
---
## 题目大意
N条绳子，长度分别为Li，从它们之中切割出K条长度相同的绳子，求这K条绳子每条最长的长度，答案保留至小数点后二位。
<!-- more -->

## 题目分析
二分搜索的经典应用，“假定一个解并判断是否可行”。现在我们假定要切割出长度为x的绳子，然后判断能否切割成K条即可。

这道题卡在最后输出答案上了，不能四舍五入，一定要向下取。

## source code
```c++
#include <cstdio>
#include <cmath>
int N, K;
double a[12000];
bool judge(double k) {
    int cnt = 0;
    for (int i = 0; i < N; i++) {
        cnt += a[i] / k;
    }
    return cnt >= K;
}
int main() {
    scanf("%d%d", &N, &K);
    for (int i = 0; i < N; i++) {
        scanf("%lf", a + i);
    }
    double L = 0.0001, R = 110000;
    for (int i = 0; i < 100; i++){
        double mid = (L + R) / 2;
        if (judge(mid)) L = mid;
        else R = mid;
    }
    
    printf("%.2lf\n", floor(L * 100) / 100);
}
```