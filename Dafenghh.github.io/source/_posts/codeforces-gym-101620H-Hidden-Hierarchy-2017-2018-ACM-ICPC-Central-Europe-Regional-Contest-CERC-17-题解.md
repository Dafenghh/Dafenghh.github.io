---
title: >-
  codeforces gym 101620H [Hidden Hierarchy] 2017-2018 ACM-ICPC, Central Europe
  Regional Contest (CERC 17) 题解
comments: true
categories: ACM
tags:
  - tree
  - dfs and similar
abbrlink: 1dd196f0
date: 2018-01-26 13:39:40
---
# 题目大意
给出n个文件的路径和大小，然后要像Windows资源管理器的侧边栏那样输出文件夹的分层结构。当一个文件夹里的所有子文件夹大小都不超过t时，它会折叠起来。


<!-- more -->


# 题目分析
模拟题，集训的时候打崩了。关键是建树的过程，将路径拆分成文件夹名的vector，然后利用这个vector创建这个路径上的所有文件夹的结点。

所有结点按创建次序保存在数组中，每一个结点包含一个map<string, int>, 存放它的子节点，“文件夹名”到结点位置的映射。

这样一来，这道题就很好写了。

# source code
```c++
#include <cstdio>
#include <string>
#include <cstring>
#include <map>
#include <vector>
#include <iostream>
using namespace std;
struct dir{
    string name;
    int sz;
    map<string, int> subdir;
}a[60000];
int tot = 0, t;

void addFile(vector<string> &path, int pi, int sz, int ai) {
    a[ai].sz += sz;
    if (pi >= path.size()) return;
    if (!a[ai].subdir.count(path[pi])) {
        a[++tot].name = path[pi];
        a[ai].subdir[path[pi]] = tot;
    }
    addFile(path, pi + 1, sz, a[ai].subdir[path[pi]]);
}

bool canFold(int ai) {
    for (auto i:a[ai].subdir) if (a[i.second].sz >= t) return false;
    return true;
}

void printDir(int ai = 0, string ps = "") {
    
    ps += a[ai].name + "/";
    if (a[ai].subdir.empty()) {
        printf("  %s %d\n", ps.c_str(), a[ai].sz);
    }
    else if (canFold(ai)) {
        printf("+ %s %d\n", ps.c_str(), a[ai].sz);
    }
    else {
        printf("- %s %d\n", ps.c_str(), a[ai].sz);
        for (auto i:a[ai].subdir) printDir(i.second,ps);
    }
}


char str[1024];
int main() {
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        int sz;
        scanf("%s%d", str, &sz);
        vector<string> path;
        string buf;
        int len = strlen(str);
        for (int i = 1; i < len; i++) {
            if (str[i] == '/') path.push_back(buf),buf.clear();
            else buf += string(1, str[i]);
        }
        addFile(path, 0, sz, 0);
    }
    scanf("%d", &t);
    printDir();
}

```