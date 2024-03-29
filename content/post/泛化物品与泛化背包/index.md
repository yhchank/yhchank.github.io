---
title: "泛化物品与泛化背包"
date: 2023-02-08T10:09:02+08:00
draft: false
---

# 泛化物品

有一种物品，它的价值随着你分配给它的费用而变化（即 $v = f(w)$，其中 $v$ 表示价值，$w$ 表示费用），这就是泛化物品

其实，01 背包、完全背包、多重背包等中的物品，都属于泛化物品

具体地：

- 在 01 背包中，对于每个物品 $a$，其价值：
  $$
  \begin{equation}
  a(w) =
  \begin{cases}
  v_i& w = w_i\\
  0& 其他情况
  \end{cases}
  \end{equation}
  $$
- 在完全背包中，对于每个物品 $a$，其价值：
  $$
  \begin{equation}
  a(w) =
  \begin{cases}
  \frac{w}{w_i}v_i& w 被 w_i 整除\\
  0& 其他情况
  \end{cases}
  \end{equation}
  $$
- 在多重背包中，对于每个物品 $a$，其价值：
  $$
  \begin{equation}
  a(w) =
  \begin{cases}
  \frac{w}{w_i}v_i& w 被 w_i 整除且 \frac{w}{w_i} \le n_i\\
  0& 其他情况
  \end{cases}
  \end{equation}
  $$

# 泛化物品的和与泛化背包

求两个泛化物品 $a$ 与 $b$ 的和就是用**给定的费用**从这两个物品中获得**最大的价值**

因为总费用确定，所以对于 $\forall v \in \{0 ... V\}$（$V$ 为背包容量/总资金数）可以枚举费用如何分配给两个泛化物品，即：
$$
f(v)=\max_{k=0}^{v}{a(k)+b(v-k)}
$$
可以发现，$f(v)$ 是一个由泛化物品 $a$ 与 $b$ 决定的定义域为 $\{0 ... V\}$ 函数，也就是说，$f$ 是一个由泛化物品 $a$ 和 $b$ 决定的泛化物品

**在一个背包问题中，若将两个泛化物品代以它们的和，不影响问题的答案**，所以对于其中的物品都是泛化物品的背包问题（**泛化背包**），**求解答案的过程本质上就是求所有这些泛化物品之和的过程**

### 关于 01 背包

在我们求解普通 01 背包时，`dp[i][]` 其实就是**前 $i$ 个泛化物品与一个虚拟物品求和之后的结果**

关于这个**虚拟物品**：因为我们不一定要将背包完全塞满，有时放弃一些空间，总价值反而会更大，那么这些剩余的空间怎么办呢？于是就有了 `dp[0][]` 这个虚拟的物品，我们求解普通 01 背包时，并没有初始化 `dp[0][]`，所以值默认都是 $0$，也就是说无论给这个“0号物品”多少空间，它的价值都是 $0$，这样就起到了占用多余空间的作用

### 关于分组背包

问题：有一个容量为 $M$ 的背包和 $N$ 组物品，每组若干个，每个物品有体积和价值两个属性；现在，对于每个组，可以选择一个物品或不选，求选出物品的最大总价值

其实分组背包也是一类典型的泛化背包，其中每一个组为一个泛化物品，每个**给定费用**所对应的**价值**由组内的物品决定

# 泛化背包的应用

**处理分配问题**：如果有多个事件，且每个事件都可以抽象出其价值与费用（也有可能是“分配的时间”等）的函数关系（通过暴力枚举，DP 等），那么泛化背包可以用于求解最大总价值

# 例题

## [[太戈 1937] 火烧藏经阁](https://www.luogu.com.cn/problem/U279659)

首先，我们可以将拿的本书视为总资金数（或背包总体积），现在要将这个数量分配给每一层

于是，可以把每一层看作一个泛化物品，暴力枚举预处理出价值与费用的函数关系，再利用泛化背包求解

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;

#define MAXN 105
#define MAXC 205
#define MAXM 10005

int n, m;
int c[MAXN];
int v[MAXN][MAXC];
int g[MAXN][MAXC];
// 将第 i 层视作一个泛化物品，计算每一个体积（拿的书的本书）所对应的价值，
// 也就是：第 i 层拿最多 j 本时，该层最多拿多少价值
int dp[MAXN][MAXM];

// 对于第 x 层，计算 g[x][]
void calc_g(int x) {
    static int s[MAXC];
    for (int w = 1; w <= c[x]; w++) {
        s[w] = s[w - 1] + v[x][w]; // 前缀和
    }
    int mn = min(c[x], m);
    for (int w = 1; w <= mn; w++) { // 枚举体积，计算对应的价值
        for (int j = 0; j <= c[x] && j <= w; j++) { // 从左边取 j 本，从右边取 w - j 本
            g[x][w] = max(g[x][w], s[j] + (s[c[x]] - s[c[x] - (w - j)]));
        }
    }
}

int main() {
    freopen("books.in", "r", stdin);
    freopen("books.out", "w", stdout);
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> c[i];
        for (int j = 1; j <= c[i]; j++) {
            cin >> v[i][j];
        }
        calc_g(i);
    }
    for(int i = 1; i <= n; i++) {
        for(int j = 1; j <= m; j++) {
            dp[i][j] = dp[i - 1][j]; // 不选此层
            int mn = min(c[i], j);
            for(int k = 1; k <= mn; k++) {
                dp[i][j] = max(dp[i][j], dp[i - 1][j - k] + g[i][k]);
            }
        }
    }
    cout << dp[n][m] << endl;
    return 0;
}
```

## 另一道例题

[P1336 最佳课题选择](https://www.luogu.com.cn/problem/P1336)
