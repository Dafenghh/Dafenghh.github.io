---
title: 'NWERC 2017 H [High Score] (The 2017 Northwestern Europe Regional Contest) 题解'
comments: true
abbrlink: 54cee541
date: 2018-01-24 10:34:18
categories:
tags:
---

# 题目大意


[题目链接](https://open.kattis.com/problems/highscore2) （可能需要翻墙）

给定a,b,c, 定义  $score = a^2 + b^2 + c^2+7\cdot \min(a,b,c)$

现在给出一个d，要将d拆成三份作为a，b，c的增量，即赋值`a += d1, b += d2, c += d3`, 满足`d1, d2, d3 >= 0, d1 + d2 + d3 = d`

求score的最大值。



<!-- more -->



# 题目分析
注意到加上d后，前面平方项的增量是O(d^2)级别，后面min(a,b,c)这项是O(d)级别，所以当d比较大时，把d全部加给a b c中的最大值即可；d比较小时，暴力一一验证即可。



# source code
```c++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
int main(){
	int n;
	cin>>n;
	for (int i=1;i<=n;++i){
		ll a,b,c,d;
		cin>>a>>b>>c>>d;
		ll sz0 = 4, sz1 = 10;
		if (a < sz0 && b < sz0 && c < sz0 && d < sz1) {
			ll ans = a*a+b*b+c*c+7*min(a,min(b,c));
			for (ll d1 = 0; d1 <= d; d1++)
			for (ll d2 = 0, d3 = d - d1 - d2; d2 <= d && d3 >= 0; d2++, d3 = d - d1 - d2) {
				ans = max(ans, (a+d1)*(a+d1)+(b+d2)*(b+d2)+(c+d3)*(c+d3)+7*min(a+d1, min(b+d2,c+d3)));
			}
			cout << ans << endl;
			continue;
		}
		ll mat=max(max(a,b),c);
		ll t1=ll(a+d)*(a+d)+(ll)b*b+(ll)c*c+ll(7)*min(min(a+d,b),c);
		ll t2=ll(a)*a+ll(b+d)*(b+d)+(ll)c*c+ll(7)*min(min(a,b+d),c);
		ll t3=ll(a)*a+ll(b)*b+ll(c+d)*(c+d)+ll(7)*min(min(a,b),c+d);
		cout<<max(t1,max(t2,t3))<<endl;
	}
	return 0;
}
```
