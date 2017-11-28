---
title: 'POJ 3061 [Subsequence] 题解'
comments: true
categories: ACM
tags:
  - 尺取法
  - binary search
abbrlink: acca6816
date: 2017-11-27 15:43:53
---
## 题目大意
给定一个长度为N的数列，以及整数S。求出和不小于S的连续子序列的长度的最小值。如果解不存在，则输出0.

## 题目分析
先求前缀和，然后就能用O(1)的时间, 求出一个区间的和。

之后用二分就能解决这道题。

但《挑战程序设计竞赛》(P146)中提供了一种更高效和简单的解法，叫“尺取法”。


## source code

### solution 1
```c++
#include <iostream>
using namespace std;
const int maxn = 100010;
typedef long long ll;
ll a[maxn], sum[maxn];
int N;
ll S;

bool judge(int len) {
    for (int i = 0; i + len <= N; i++) {
        if (sum[i + len] - sum[i] >= S) return true;
    }
    return false;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    while (T--) {
       cin >> N >> S;
       for (int i = 0; i < N; i++) cin >> a[i];
       sum[0] = 0;
       for (int i = 0; i < N; i++) sum[i + 1] = sum[i] + a[i];
       if (sum[N] < S) {
           cout << 0 << endl;
           continue;
       } 
       int L = 0, R = N;
       while (L + 1 < R) {
           int mid = (L + R) / 2;
           if (judge(mid)) R = mid;
           else L = mid;
       }
       cout << R << endl;
    }
}
```

### solution 2
```c++
#include <iostream>
#include <algorithm>
using namespace std;
const int maxn = 100010;
typedef long long ll;
ll a[maxn];
int N;
ll S;


int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    while (T--) {
       cin >> N >> S;
       for (int i = 0; i < N; i++) cin >> a[i];
       int s = 0, t = 0;
       ll sum = 0;
       int ans = N + 1;
       for (;;) {
           while (t < N && sum < S) sum += a[t++];
           if (sum < S) break;
           ans = min(ans, t - s);
           sum -= a[s++];
       }
       if (ans > N) ans = 0;
       cout << ans << endl;       
    }
}
```
