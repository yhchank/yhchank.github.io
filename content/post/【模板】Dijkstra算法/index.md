---
title: 【模板】Dijkstra算法
date: 2022-08-09T21:06:28+08:00
tags: [C++/算法]
---

## 朴素算法

```cpp
void dijkstra() {
	for (int i = 1; i <= n; i++) {
		int mink = 0;
		for (int j = 1; j <= n; j++) {
			if (!vis[j] && d[j] <= d[mink]) {
				mink = j;
			}
		}
		vis[mink] = true;
		for (Edge &e : g[mink]) {
			if (d[mink] < d[e.v] && d[mink] + e.w < d[e.v]) {
				d[e.v] = d[mink] + e.w;
			}
		}
	}
}
```

## 堆优化

```cpp
void dijkstra() {
	while (!q.empty()) {
		int x = q.top().id;
		q.pop();
		if (!vis[x]) {
			vis[x] = true;
			for (Edge &e : g[x]) {
				if (d[e.v] > d[x] + e.w) {
					d[e.v] = d[x] + e.w;
					if (!vis[e.v]) {
						q.push({e.v, d[e.v]});
					}
				}
			}
		}
	}
}
```
