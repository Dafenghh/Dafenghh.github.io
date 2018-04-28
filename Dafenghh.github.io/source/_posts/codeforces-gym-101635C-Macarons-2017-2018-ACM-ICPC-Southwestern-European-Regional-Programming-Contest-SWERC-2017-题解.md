---
title: >-
  codeforces gym 101635C [Macarons] 2017-2018 ACM-ICPC, Southwestern European
  Regional Programming Contest (SWERC 2017) 题解
comments: true
abbrlink: 9049154c
date: 2018-01-26 15:22:09
categories:
tags:
---
# 题目大意
用`1*1`或者`1*2`的小长方形完整覆盖N*M的长方形（不可重叠），问有多少种覆盖方式？



<!-- more -->

# 题目分析

行数很少，`N <= 8`, 列数很多，`M <= 10^18`

对于每一列，用一个长度为N的二进制串表示它的状态，0表示这一个位置放置着`1*1`的方块，1表示这一个位置放置着`1*2`的方块。

构造这样一个转移矩阵，`T[i][j]`表示状态i的列右边连着状态j的列的时候，有多少种状态j的列是可行的。

然后对矩阵求快速幂即可。


# source code
```c++
#include <bits/stdc++.h>
using namespace std;
const int mod = 1e9;
int get(){
    char ch; int v,f=0;
    while (!isdigit(ch=getchar())) if (ch=='-') break;
    if (ch=='-') f=1;else v=ch-48;
    while (isdigit(ch=getchar())) v=v*10+ch-48;
    return f?-v:v;
} 
int n;
typedef long long ll;
const int maxn =300;
ll mat[maxn][maxn], M,sz;
int trans(int X, int Y)
{
    int dp[10][2];
    dp[0][0]=1, dp[0][1]=0;
    for (int i = 1, bin = 1; i <= n; i++, bin <<= 1)
    {
        int x = ((X & bin) > 0), y = ((Y & bin) > 0);
        if (y == 1) {
            dp[i][0] = dp[i-1][1] + (x==0?dp[i-1][0]:0);
            dp[i][1] = dp[i-1][0];
        }
        else {
            dp[i][0] = dp[i-1][0];
            dp[i][1] = 0;
        }
    }
    return dp[n][0];
}
long long tmp[maxn][maxn],Map[maxn][maxn];
int ksm(ll m[maxn][maxn],ll t,int N){
    memcpy(Map,m,sizeof(Map));
    t--;
    while (t){
        if (t%2==1){
        for (int i=1;i<=N;i++) for (int j=1;j<=N;j++) tmp[i][j]=0;//清空临时数组
          for (int i=1;i<=N;i++)
            for (int k=1;k<=N;k++)
            if (m[i][k])
                for (int j=1;j<=N;j++) tmp[i][j]+=m[i][k]*Map[k][j]%mod,tmp[i][j]%=mod; //矩阵乘法
          for (int i=1;i<=N;i++)
            for (int j=1;j<=N;j++) Map[i][j]=tmp[i][j];      //赋值到原数组
        }  
        for (int i=1;i<=N;i++) for (int j=1;j<=N;j++) tmp[i][j]=0;//清空临时数组
           for (int i=1;i<=N;i++)
            for (int k=1;k<=N;k++)
            if (m[i][k])
                for (int j=1;j<=N;j++) tmp[i][j]+=m[i][k]*m[k][j]%mod,tmp[i][j]%=mod;
          for (int i=1;i<=N;i++)
             for (int j=1;j<=N;j++) m[i][j]=tmp[i][j];     
        t>>=1;

    }

    ll ans=0;
    for (int i=1;i<=N;++i) ans=(ans+Map[N][i])%mod;
    return ans;
}
int main(){
    cin >> n >> M;
    sz = (1 << n);
    for (int i = 1; i <= sz; i++)
    for (int j = 1; j <= sz; j++) {
        mat[i][j] = trans(i-1, j-1);
    }
    cout << ksm(mat,M,sz)<<endl;
    return 0;
}
```
