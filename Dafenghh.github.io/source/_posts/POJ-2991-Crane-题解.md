---
title: 'POJ 2991 [Crane] 题解'
comments: true
categories: ACM
tags:
  - segment tree
  - geometry
  - math
abbrlink: a9a98564
date: 2017-12-29 19:03:22
---

# 题目大意
N条线段首尾相连，初始时全都垂直于x轴呈一直线。标号从下往上，1到N。C次询问，每次询问给出S和A，将第S条线段和第S+1条线段的角度修改成A，角度指的是从S开始沿逆时针方向旋转到S+1经过的角度。求每次询问时第N条线段的末端点坐标。

<!-- more -->

# 题目分析
《挑战程序设计竞赛》P170 例题，线段树的经典应用，很启发思维！

如何转化成线段树问题呢？

其实也是一种分治的思想。用v(i,j)表示第i条线段的始点到第j条线段的终点的向量，那么题目要求的就是每次更新后的v(1, n)
如何求得v(1, n)呢？分而治之，将区间折半，v(1, n) = v(1, n / 2) + v(n / 2 + 1, n),。

如果我们旋转第i条线段，那么显然i到n的线段坐标都要变化。这样一来，我们就需要对这所有的点更新坐标一次，很明显效率是不足够的。

我们需要将旋转的特征提取出来，在上面这个例子中，(i..n)的点的相对位置是不变的，只是整体发生了旋转。

所以，我们可以只记录一个整体旋转的角度$\alpha$ （相对于竖直方向的角度增量，逆时针为正），外加修改原来v(i, j)的定义，变成将第i条线段旋转至与地面垂直的时候，第i条线段的始点到第j条线段的终点的向量。

这样一来，我们求v(i,j)的实际值时，乘上$\alpha$对应的旋转变换矩阵即可：
\begin{bmatrix} \cos \alpha & -\sin \alpha\\\ \sin \alpha & \cos \alpha \end{bmatrix}

所以构造这样一棵线段树，每个结点表示一段连续的线段区间，维护这两个值：

将线段区间的第一条线段旋转至垂直方向后，第一条线段的起点到最后一条线段的终点的向量 (v[i])
两个儿子连接后，右儿子需要旋转的角度 (ang[i])
记第i个结点的左右儿子结点为chl,chr，那么
v[i] = v[chl] + M(ang[i]) * v[chr]

P.S. WA了几发，原因是：当更新角度的线段s在当前区间mid之前时，我没有更新当前区间的ang值。后来发现，区间ang值的增量跟线段s角度增量一致。其实这个也很好理解（理解不了在图上画个三角形来旋转也能证明出来），因为旋转变换其实是对整个坐标系旋转，所以点旋转的是相同角度，向量旋转也是相同角度（注意平移变换不改变角度）。

P.S. 这题POJ有坑，输出格式是假的，样例中间不用加空行。

# source code
```c++
#include <cstdio>
#include <cmath>
using namespace std;

const double PI = acos(-1.0);
const int maxn = 100010;
double vx[2 * maxn], vy[2 * maxn], ang[2 * maxn], prv[maxn];
int len[maxn];
int n, q;

void init(int v, int l, int r) {// [l, r]
    if (l >= r) return;
    ang[v] = 0;
    vx[v] = 0;
    if (l + 1 == r) {
        vy[v] = len[l];
        return;
    }
    int chl = 2 * v + 1, chr = 2 * v + 2, mid = (l + r) / 2;
    init(chl, l, mid);
    init(chr, mid, r);
    vy[v] = vy[chl] + vy[chr];
}

void update(int s, double a, int v, int l, int r) { // angle changes a
    if (s <= l ||s >= r) return;
    int chl = 2 * v + 1, chr = 2 * v + 2, mid = (l + r) / 2;
    update(s, a, chl, l, mid);
    update(s, a, chr, mid, r);
    if (s <= mid) ang[v] += a;
    double Co = cos(ang[v]), Si = sin(ang[v]);
    vx[v] = vx[chl] + Co * vx[chr] - Si * vy[chr];
    vy[v] = vy[chl] + Si * vx[chr] + Co * vy[chr];
}


int main() {
    int testcnt = 0;
    while (scanf("%d%d", &n, &q)!=EOF) {
        testcnt++;
        for (int i = 0; i < n; i++) scanf("%d", len + i);
        init(0, 0, n);
        for (int i = 0; i < n; i++) prv[i] = PI;

        while (q--) {
            int s, angle_360;
            scanf("%d%d", &s, &angle_360);
            double a = (double)angle_360 / 180 * PI;
            update(s, a - prv[s], 0, 0, n);
            prv[s] = a;
            printf("%.2lf %.2lf\n", vx[0], vy[0]);
        }
    }
}
```