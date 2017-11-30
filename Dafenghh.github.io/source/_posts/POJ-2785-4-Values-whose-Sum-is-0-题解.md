---
title: 'POJ 2785 [4 Values whose Sum is 0] 题解'
comments: true
date: 2017-11-30 22:45:20
categories: ACM
tags:
- binary search
---
## 题目大意
大小都为N的四个数组`A[], B[], C[], D[]`, 从每个数组中分别选出一个数，`a, b, c, d` , 使得 `a + b + c + d = 0 `，问有多少种选择方式。

<!-- more -->


## 题目分析
《挑战程序设计竞赛》P160例题。《挑战》通过这道题介绍了重要的思想方法，即“折半枚举”的方法，将问题一拆为二进行枚举。

枚举出`A[i]+B[j]`的所有可能值和`C[i]+D[j]`的所有可能值, 之后用二分查找互为相反数的两个值的组数即可。


## source code
```c++
#include <iostream>
#include <algorithm>

using namespace std;
const int maxn = 4200;
typedef long long ll;
int a[maxn], b[maxn], c[maxn], d[maxn];
int ab[maxn * maxn], cd[maxn * maxn];
ll ans = 0;
int main() {
	ios::sync_with_stdio(false);
	cin.tie(0);
	int n;
	cin >> n;
	for (int i = 0; i < n; i++) cin >> a[i] >> b[i] >> c[i] >> d[i];
	for (int i = 0; i < n; i++)
	for (int j = 0; j < n; j++)
		ab[i * n + j] = a[i] + b[j], cd[i * n + j] = c[i] + d[j];
	sort(cd, cd + n * n);
	for (int i = 0; i < n * n; i++)
		ans += upper_bound(cd, cd + n * n, -ab[i]) - lower_bound(cd, cd + n * n, -ab[i]);
	cout << ans << endl;
	

}

```