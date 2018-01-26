---
title: >-
  codeforces gym 101630B [Box] 2017-2018 ACM-ICPC, Northern Eurasia
  (Northeastern European Regional) Contest (NEERC 17) 题解
comments: true
categories: ACM
tags:
  - math
abbrlink: 5f7aa68
date: 2018-01-23 20:47:49
---
# 题目大意
给定一张`w*h`大小的纸片，要求裁剪出一个`a*b*c`的长方体的展开图，问是否可行。



<!-- more -->


# 题目分析
长方体展开图共有11种情况，分别算出每种情况所需要的长宽，然后枚举验证即可。


P.S. 小学奥数题的升级版。一开始用蛮力想象展开图平起来后的边的情况。但毕竟自己的空间想象能力并不是很强，而且这样很费时，所以并不是好的做法。其实只要在展开图中根据相邻关系标上边的长度即可。

P.S. opentrain的测试数据很弱，集训时1A。回来交CF，WA了第40个点。改了一个小时后，WA第55个点。原来自己的next_permutation用在了原边长数组上，犯了一个低级错误。


# source code
```c++
#include <cstdio>
#include <algorithm>
using namespace std;
int A[3],inx[3]={0,1,2};
	int w, h;
bool comp(int a, int b) {
	if (a <= w && b <= h) return true;
	if (b <= w && a <= h) return true;
	return false;
}
int main() {
	for (int i = 0; i < 3; i++) scanf("%d", A + i);
	
	scanf("%d%d", &w, &h);
	bool find = false;
	do {
		int a = A[inx[0]], b= A[inx[1]], c = A[inx[2]];
		if (comp(2*(a+c), b + 2 * c)||
			comp(3*b+a+c,a+c)|| 
			comp(a+b+c,a+b+2*c)|| 
			comp(a+b+c,2*b+2*c)||
			comp(a+2*b+c,a+2*c)) find = true;
	}while (!find && next_permutation(inx, inx + 3));
	puts(find?"Yes":"No");
}
```