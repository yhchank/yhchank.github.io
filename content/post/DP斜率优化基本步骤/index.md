---
title: "DP斜率优化基本步骤"
date: 2023-04-08T09:51:42+08:00
draft: false
---

1. 写出转移朴素 dp 转移方程，如 `min{dp[j] + (i - j) * (i - j - 1) / 2} + a[i]`（随便举一个例子）
2. 将只和 $j$ 有关的项放在一起，同时和 $i$、$j$ 有关的项放在一起，之和 $i$ 有关的项提到 `min{}` 或 `max{}` 外面（如果无法直接变形，可以先展开）：`min{dp[j] + j * (j + 1) / 2 - j * i} + i * (i - 1) / 2 + a[i]`
3. 确定每条直线的斜率和截距，在这个例子中，第 $i$ 条直线的斜率为 `-i`，截距为 `dp[i] + i * (i + 1) / 2`
4. 套斜率优化模板，如下（将 `k<i>`、`b<i>` 和 `rest<i>` 分别替换为推导出的斜率如 `-i`、截距如 `dp[i] + i * (i + 1) / 2` 和 `{}` 外的式子如 `i * (i - 1) / 2 + a[i]`）

``` cpp
struct Line {
    int k, b;
    int y(int x) {
        return k * x + b;
    }
};

int ql, qr;
Line q[MAXN]; // 可用直线队列（q[l ... r]）

// 求直线交点横坐标
#define X(la, lb) -(double)((la).b - (lb).b) / ((la).k - (lb).k)

ql = 1;
qr = 0;
q[++qr] = (Line) {
    k<0>,   // 斜率
    b<0>,   // 截距
};
for (int i = 1; i <= n; i++) {
    // 左删
    while (qr > ql && X(q[ql], q[ql + 1]) < i) {
        ql++;
    }
    // 计算 dp[i]
    dp[i] = q[ql].y(i) + rest<i>;
    Line lnow = (Line) {
        k<i>,   // 斜率
        b<i>    // 截距
    };
    // 右删
    while (qr > ql && X(q[qr], q[qr - 1]) > X(lnow, q[qr - 1])) {
        qr--;
    }
    q[++qr] = lnow;
}
```