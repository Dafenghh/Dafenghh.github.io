---
title: 'POJ 2155 [Matrix] 题解'
comments: true
date: 2018-01-09 22:15:40
categories: ACM
tags:
  - BIT
---
# 题目大意
一个N*N的布尔矩阵，初始值全为0，T次询问，两种操作：
1. 查询某个点的值
2. 将一个矩形方块的布尔值全部翻转。

<!-- more -->

# 题目分析
二维BIT的简单题目。一维BIT很容易扩展成二维，直接加多一层循环即可，单次操作复杂度由$O(\log n)$变成$O(\log^2 n)$

区间更新转化为4个端点的更新即可。

由于每个点只有两种状态，所以用异或处理非常方便。

# source code
```c++
#include <cstdio>
#include <cstring>
const int maxn = 1020;
int bit[maxn][maxn], n, q;
void init() {
    memset(bit, 0, sizeof(bit));
}
int lowbit(int i) {
    return i & -i;
}
int sum(int x, int y) { // [1..x, 1..y]
    int res = 0;
    for (int i = x; i > 0; i -= lowbit(i))
    for (int j = y; j > 0; j -= lowbit(j))
        res ^= bit[i][j];
    return res;
}

void add(int x, int y) {
    for (int i = x; i <= n; i += lowbit(i))
    for (int j = y; j <= n; j += lowbit(j))
        bit[i][j] ^= 1;
}

void update(int x1, int y1, int x2, int y2) {
    add(x1, y1);
    add(x1, y2 + 1);
    add(x2 + 1, y1);
    add(x2 + 1, y2 + 1);
}


int main() {
    int T;
    scanf("%d", &T);
    while (T--) {
        scanf("%d%d", &n, &q);
        init();
        while (q--) {
            char s[10];
            scanf("%s", s);
            if (s[0] == 'Q') {
                int x,y;
                scanf("%d%d", &x, &y);
                printf("%d\n", sum(x, y));
            }
            else {
                int x1, y1, x2, y2;
                scanf("%d%d%d%d", &x1, &y1, &x2, &y2);
                update(x1, y1, x2, y2);
            }
        }
        if (T) printf("\n");
    }
}


```