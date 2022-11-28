---
title: 【算法笔记】ST表
date: 2022-09-12T18:42:17+08:00
tags: [C++/算法]
---

# ST表

### 处理的问题

**RMQ：** 区间最值问题

### 适用情况

一次初始化（$\Theta(n \log n)$），$q$次询问（$q$极大，每次$\Theta(1)$）

### 流程

1. **倍增**思想预处理出每个`以i为左边界，长度为2的j次方的区间最值`
2. $\Theta(1)$查询

### 模版（以max举例）

```cpp
int lg[MAXN];
int st[MAXN][35];

void init_lg() { // 初始化log值
	for(int i = 2; i <= n; i++) {
		lg[i] = lg[i >> 1] + 1;
	}
}

void init_st() { // 初始化st表
	for(int i = n; i >= 1; i--) {
		st[i] = a[i];
		for(int k = 1; i + (1 << k) <= n; i++) {
			st[i][k] = max(st[i][k - 1], st[i + (1 << (k - 1))][k - 1]);
		}
	}
}

void st_q(int l, int r) { // 区间查询
	int s = lg[r - l + 1];
	return max(st[l][s], st[r - (1 << s) + 1][s]);
}
```

### 可以使用ST表的问题

只有**可重复贡献问题**可以使用**ST表**，所谓**可重复贡献问题**，是指满足`x opt x == x`的操作`opt`

### 注意点

**第一层循环一定是**`for(int i = n; i >= 1; i--)`**，方向不能错**
