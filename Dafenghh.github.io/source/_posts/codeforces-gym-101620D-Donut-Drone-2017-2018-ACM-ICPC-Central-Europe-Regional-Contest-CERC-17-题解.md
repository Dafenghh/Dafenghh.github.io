---
title: >-
  codeforces gym 101620D [Donut Drone] 2017-2018 ACM-ICPC, Central Europe
  Regional Contest (CERC 17) 题解
comments: true
categories:
  - ACM
tags:
  - segment tree
  - data structures
abbrlink: a74fadaa
date: 2018-01-21 20:41:28
---
# 题目大意
一个矩形方块，有r*c个格子，水平和垂直方向上都可以将它视为首尾相连的（也即从最右边向右移会回到最左边，从最下边向下移会回来最上边，以此类推）。每个格子上有一个数。每一次移动会往相邻的右边、右上、右下的三个格子中选最大数的格子移动。初始位置在左上角。两种操作：1. 移动k步，输出新位置；2.修改某个格子的数。


数据范围：r，c $\leq$ 2000, 询问数5000以内。 

<!-- more -->

# 题目分析
移动k步时，列数的增量是确定的（k），行数未知，由二维数组确定。

可以很自然地想到用线段树解决这个问题。线段树的每一个结点，维护的是一张从第l列到第r列, 即[L,r)，行数y的转移表（本来行数应该用x来表示，但是一开始敲时就弄混了，所以后面只好交换了x和y的定义）。

这样，当我们从第y行第x列出发时，若k比较小直接模拟。k比较大时，先查询线段树，拿到y在[x, c)的转移值y'。这样就回到了(y',0) 即第一列。

然后一次走c步，即采用[0,c)的转移表，意思就是从第一列一直往右走，知道走回第一列。不断重复这个过程，每一次可以使步数+c。


理论上这么模拟可以使单次询问的复杂度达到O(k/c*log c);

但考虑到行数只有r个，所以只会产生长度不超过r的环。所以把经过的点记录一下，产生环即跳出。这样复杂度可以优化至O(r*log c).


# source code
```c++
#include <cstdio>
#include <iostream>
#include <cctype>
#include <cstring>
using namespace std;
const int maxn = 2010;
const int INF = 1234567890;
int dat[4 * maxn][maxn], row, col , a[maxn][maxn], NY[maxn][maxn],circle[maxn],posi[maxn];

int query(int y, int x1, int x2, int v = 0, int l = 0, int r = col) { // [l, r)
	 if (x2 <= l || r <= x1) return y;
	 if (x1 <= l && r <= x2) return dat[v][y];
	 int mid = (l + r) >> 1, chl = v * 2 + 1, chr = v * 2 + 2;
	 int LAns = query(y, x1, x2, chl, l, mid);
	 return query(LAns, x1, x2, chr, mid, r);
}

void update(int x, int y, int val, int v = 0, int l = 0, int r = col) {
	 if (x < l || x >= r) return;
	 if (l + 1 == r) {
			dat[v][y] = val;
			return;
	 }
	 int mid = (l + r) >> 1, chl = v * 2 + 1, chr = v * 2 + 2;
	 update(x, y, val, chl, l, mid);
	 update(x, y, val, chr, mid, r);
	 for (int i = 0; i < row; i++) dat[v][i] = dat[chr][dat[chl][i]];
}

void build(int v = 0, int l = 0, int r = col) {
	if (l + 1 == r) {
		for (int i = 0; i < row; i++) dat[v][i] = NY[l][i];
		return;
	}
	int mid = (l + r) >> 1, chl = v * 2 + 1, chr = v * 2 + 2;
	build(chl, l, mid);
	build(chr, mid, r);
	for (int i = 0; i < row; i++)
		dat[v][i] = dat[chr][dat[chl][i]];
}
int read(){
	int v; char ch;
	while (!isdigit(ch=getchar()));
	v=ch-48;
	while (isdigit(ch=getchar())) v=v*10+ch-48;
	return v;
}

int getNY(int x, int y) {
	int nx = (x + 1) % col, Max = -INF, res;
	for (int dy = -1; dy <= 1; dy++){
		int ny = (y + dy + row) % row;
		if (a[nx][ny] > Max) {
			Max = a[nx][ny];
			res = ny;
		}
	}
	return res;
}

int main() {
	row = read(), col = read();
	for (int i = 0; i < row; i++)
		 for (int j = 0; j < col; j++) a[j][i] = read();
	for (int i = 0; i < row; i++)
		 for (int j = 0; j < col; j++) NY[j][i] = getNY(j, i);
	 

	build();
	int m = read();

	int y = 0, x = 0;
	while (m--) {
		char s[10];
		scanf("%s",s);
		if (s[0] == 'm') {
			int k = read();
			if (k > col) {
				if (x != 0) k-= col-x, y = query(y,x,col),x=0;
				memset(posi, -1, sizeof(posi));
				circle[0] = y;
				posi[y] = 0;
				int ci = k / col;
				k -= ci * col;
				for (int i = 1; i <= ci; i++)
				{
					circle[i] = y = query(y, 0, col);
					if (posi[y] != -1) {
						int len = i - posi[y];						
						y = circle[posi[y] + (ci - posi[y]) % len];
						break;
					}
					posi[y] = i;
				}
				
				if (k > 0) y = query(y, 0, k);
				x = k;
			}
			else {
				for (int i = 0; i < k; i++){
					y = NY[x][y];x = (x + 1) % col;
				}
			}
			printf("%d %d\n", y + 1, x + 1);
		}
		else {
			int cy = read() - 1, cx = read() - 1, val = read();
			a[cx][cy] = val;
			int px = (cx + col - 1) % col;
			for (int dy = -1; dy <= 1; dy++)
			{
				int py = (cy + dy + row) % row;
				int temp = getNY(px, py);
				if (NY[px][py] != temp) {
					NY[px][py] = temp;
					update(px, py, temp);
				}
			}
		}
	}
}

```