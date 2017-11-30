---
title: >-
  codeforces gym 101612 C [Consonant Fencity] (ICPC 2017-2018 NEERC Northern
  Subregional Contest St Petersburg November 4 2017) 题解
comments: true
categories: ACM
tags:
  - brute force
  - 位运算
abbrlink: 9c682bc0
date: 2017-11-29 22:35:36
---

## 题目描述
gym 101612 C题
链接： [gym](http://codeforces.com/gym/101612)


定义辅音字母为除了{a,e,i,o,u,w,y}之外的19个字母。
然后定义一个字符串的fencity为串中有多少对相邻的辅音字母，且它们一个大写一个小写。
给出一个只包含小写字母的字符串，现在你要指定19个辅音字母中的若干个字母，将字符串中的这些字母全部转换为大写。求fencity最大的串。

<!-- more --> 

## 题目分析
一共有19个字母要考虑。将这19个字母想象成图中的点。我们先扫一遍字符串，当遇到辅音字母相邻时，就将这两个辅音字母的边权+1.

然后这题就是说，往19个点染两种颜色（大写或者非大写），当一条边的两个端点颜色不一样时，这条边的边权生效。求边权和最大值。

由于对称性，我们可以固定一个点的颜色，然后只有18个点需要考虑，那么就是`2^18`中情况需要枚举。每个情况扫一遍所有的边，那么就是`2^18 * 19 * 18 / 2`的复杂度，大概为4500万，所以暴力方式能够解决此题。

P.S.有个小插曲，`(1 << M[s[i]])) > 0` 忘了加前面的括号，debug了好长时间。
下次打比赛时要先打印一份C/C++运算符优先级列表，然后将它压在台面上。

## source code
```c++
#include <cstdio>
#include <cstring>
#include <map>
#include <set>
using namespace std;
int n = 19;
int w[20][20];
int fencity[(1 << 18)];

void solve() {
    int xn = (1 << (n - 1));
    for (int x = 0; x < xn; x++) {

        fencity[x] = 0;
        for (int i = 0; i < n; i++)
        for (int j = i + 1; j < n; j++){
            int p = (x & (1 << i));
            int q = (x & (1 << j));
          
            if (p == 0 && q > 0 || p > 0 && q == 0){
               
                fencity[x] += w[i][j];
            } 
        }
    }
}

char s[1000010];
char voewls[] = "aeiouwy";
int main() {
    #ifndef LOCAL
    freopen("consonant.in", "r", stdin);
    freopen("consonant.out", "w", stdout);
    #endif
    scanf("%s", s);
    int len = strlen(s);
    set<char> S(voewls, voewls+ 7);
    map<char, int> M;
    int cnt = 0;
    for (char ch = 'b'; ch <= 'z'; ch++) {
        if (!S.count(ch)) M[ch] = cnt++;
    }

    for (int i = 0; i < len - 1; i++) {
        if (M.count(s[i]) && M.count(s[i + 1])) {
            w[M[s[i]]][M[s[i + 1]]]++;
            w[M[s[i + 1]]][M[s[i]]]++;
        }
    }
    solve();
    int ans = 0, xn = (1 << (n - 1));
    for (int x = 1; x < xn; x++) if (fencity[ans] < fencity[x]) ans = x;

    for (int i = 0; i < len; i++) {
        if (!S.count(s[i]) && ((ans & (1 << M[s[i]])) > 0)) printf("%c", s[i] + 'A' - 'a');
        else printf("%c", s[i]);
    }
    printf("\n");

}

```