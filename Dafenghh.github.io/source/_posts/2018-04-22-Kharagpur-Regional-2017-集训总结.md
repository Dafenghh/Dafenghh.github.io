---
title: 2018.04.22 Kharagpur Regional 2017 集训总结
comments: true
categories: ACM
tags:
  - summary
abbrlink: 9ffddd26
date: 2018-04-29 20:13:01
---

# Summary

赛中通过3题，FDK.  F是暴力水题，D和K一共7记罚时，做得不好。D的错误是考虑错了复杂度，所以超时，加上输出%lld写成%d（经典错误）。K的错误是思考不谨慎，连边时加了一个多余的判断条件。

赛后没把H想出来。已经想得差不多，不过看错数据范围。一直以为要O(n^2)算法，其实是O(n^3). 要是看仔细点说不定就能想出来。（经典DP套路，请把训练指南的DP章节刷一遍，锻炼思维！）

赛后补了AHJ。还差EIB。加油，补完！
- [x] A
- [x] J
- [ ] B
- [ ] E
- [ ] I

<!-- more -->

## A Science Fair
### 题目大意

有N个学生，住在不同的地方。现在学校要选学生去参加科技节活动。每个学生被选去的概率的百分数等于他们的百分制成绩。

大巴在S处出发，先去接送学校选去的学生，然后开到终点F。由于学生太过活跃，当大巴经过了他们家时，他们就算没被学校选中也会强行上车。每个学生有一个talkativeness，一次旅途的talkativeness等于所有上了车的学生的talkativeness的乘积。一次旅途的cost定义如下：

 cost(Trip) = Length(Trip) + (Talkativeness(Trip) % (1e9+7）)
 
 司机会选择一条使cost最小的路线行驶。问cost的期望。
 
### solution

扩展版旅行商问题。
关键是先重构图。压缩至只有S，F跟学生所在点。而这些点间的边权应该定义为原图中不经过别的学生的最短路长度。用$n^2$次dijkstra重构图完成后，就是一个旅行商问题了。之后状态压缩DP解决。

`dp1[x][i]`表示当前走到第i个点，访问过的点集为x的最小cost。
`dp2[x]`表示访问学生集合为x以及x的超集的最小cost。这就是最终答案了。

见[Science Fair题解](a5930d20.html)



## B Black Discs
## C Uniform Strings
## D SAD Queries
## E Chef and XOR Queries
## F Taxi Making Sharp Turns
## G Spam Classification Using Neural Net
## H Non Overlapping Segments
## I Spanning Tree

## J Generating A Permutation
### 题目大意
对一个排列P，[p1, p2, ..., pN-1, pN], 定义 f(P) = max(p1, p2) + max(p2, p3) + ... + max(pN-1, pN)

给出N，K。构造排列P， 使f(P) = K. 如果不存在，就输出-1.

### solution
f(P)的值是N-1个数的和。而原排列中的数，最多贡献两次答案。

考虑排列是正序时，f(P)取到最小值，为2+3+...+N。也即2到N的数，每个数贡献1次。
最大值应该是在最大的那些数都贡献2次时取到。

怎么让最大那个数贡献2次？只需要不把它放两端就可以了。

另外注意到，一个大的数a多贡献一次的同时，就会剥夺另外一个较小的数b的贡献机会，这时候就会使答案增加a-b.

所以这道题的构造方法就是，设a为下一个贡献多一次的数，b为下一个被剥夺贡献机会的数。考虑当前的和，如果加上b-a比答案大，我们就让b大一些，直到满足条件。然后把刚刚考虑过的b依次append到答案数组，再append a就行。


见[Generating A Permutation题解](75299267.html)

## K Number Game



