---
title: 'POJ 3279 [Fliptile] 题解'
comments: true
categories: ACM
tags:
  - brute force
  - 位运算
  - 反转
abbrlink: bdca6a8a
date: 2017-11-29 22:39:42
---
## 题目大意
一个游戏，M*N的方格，每个格子可以翻转正反面，一面白色，一面黑色。当翻转一个格子时，它的相邻格子都会被翻转。用最小的翻转次数使所有格子变成白色。

<!-- more -->


## 题目分析
考虑(1,1)这个格子，翻转(1,1) (1,2) (2,1)三个格子都能改变它的状态。
但当我们确定第一行的操作后，就只有翻转(2,1)能够改变(1,1)的状态。

所以，如果我们指定第一行的操作，就能根据(1,1)的状态来确定(2,1)是否需要翻转。同样的，也能确定余下所有方格是否需要翻转。最后确定一下最下一行是否全部变成白色即可。

所以这道题，就枚举第一行的所有操作即可。

## source code
```c++
#include <iostream>
using namespace std;
int m,n;
const int maxn = 20;
int a[maxn][maxn], flip[maxn][maxn], ans_flip[maxn][maxn], stat[maxn][maxn];
int ans = 1000;

#define Forij \
    for (int i = 0; i < m; i++)\
    for (int j = 0; j < n; j++)

void F(int x, int y) {
    stat[x][y] = 1 - stat[x][y];
    int d[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    for (int di = 0; di < 4; di++){
        int nx = x + d[di][0], ny = y + d[di][1];
        if (nx >= 0 && nx < m && ny >= 0 && ny < n) stat[nx][ny] = 1 - stat[nx][ny];
    }
}

int main() {
    cin >> m >> n;
    Forij cin >> a[i][j];
    
    for (int x = 0; x < (1 << n); x++) {
        bool flag = true, cnt = 0;
        Forij stat[i][j] = a[i][j], flip[i][j] = 0;

        for (int xi = 0; xi < n; xi++){
            if (x & (1 << xi)) {
                F(0, xi);
                flip[0][xi] = 1;
                cnt++;
            }
        }

        Forij
        {
            if (i == m - 1){
                if (stat[i][j]){
                    flag = false;
                    break;
                }
            }
            else {
                if (stat[i][j]){
                    F(i + 1, j);
                    flip[i + 1][j] = 1;
                    cnt++;
                }
            }
        }

        if (flag && (cnt < ans)) {
            ans = cnt;
            Forij ans_flip[i][j] = flip[i][j];
        }
    }
    if (ans == 1000) cout << "IMPOSSIBLE" << endl;
    else{
        Forij cout << ans_flip[i][j] << (j == n - 1 ? '\n' : ' ');
    }

}

```
