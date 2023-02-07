---
title: "树的直径及求法"
date: 2023-02-02T09:28:59+08:00
draft: false
---

# 定义

**树的直径**：树上距离最长的两点之间的距离

# 求解

## 方法1：树形 DP

对于每个点，计算它的子树中到它**距离最远的点的距离** $d_1$ 以及**次远的点的距离** $d_2$，则直径为所有 $d_1 + d_2$ 的最大值

### 实现

```cpp
#include <bits/stdc++.h>
using namespace std;

#define MAXN 10005

int n;
int d1[MAXN];
int d2[MAXN];
int d;
vector<int> g[MAXN];

void dfs(int x, int fa) {
    d1[x] = 0;
    d2[x] = 0;
    for (int v : g[x]) {
        if (v == fa) {
            continue;
        }
        dfs(v, x);
        int t = d1[v] + 1;
        if (t > d1[x]) {
            d2[x] = d1[x];
            d1[x] = t;
        } else if (t > d2[x]) {
            d2[x] = t;
        }
    }
    d = max(d, d1[x] + d2[x]);
}

int main() {
    cin >> n;
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    dfs(1, 0);
    cout << d << endl;
    return 0;
}
```

## 方法2：两次 DFS

第一次 DFS：以任一节点 $A$ 为根 DFS，找到距离 $A$ 最远的节点 $B$

第二次 DFS：以 $B$ 为根节点 DFS，找到距离 $B$ 最远的节点 $C$

则 $BC$ 为树的直径

### 简单证明

第一次 DFS：一定存在某条直径包含节点 $B$，否则一定可以更长

第二次 DFS：既然有一条直径包含 $B$，那么这条直径的另一端一定是距离 $B$ 最远的节点 $C$

### 实现

```cpp
#include <bits/stdc++.h>
using namespace std;

#define MAXN 10005

int n;
vector<int> g[MAXN];
int d[MAXN];

void dfs(int x, int f) {
    d[x] = d[f] + 1;
    for (int v : g[x]) {
        if (v != f) {
            dfs(v, x);
        }
    }
}

int main() {
    cin >> n;
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    d[0] = -1;
    dfs(1, 0);
    int x = max_element(d + 1, d + n + 1) - d;
    dfs(x, 0);
    int y = max_element(d + 1, d + n + 1) - d;
    cout << d[y] << endl;
}
```

