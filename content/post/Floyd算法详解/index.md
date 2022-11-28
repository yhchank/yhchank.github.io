---
title: Floyd算法详解
date: 2022-08-06T15:47:58+08:00
tags: [C++/算法]
---
Floyd本质上使用了DP思想，我们定义$d[k][x][y]$为**允许经过前k个节点时，节点x与节点y之间的最短路径长度**，显然初始值应该为$d[k][x][y] = +\infin (k, x, y\in[1, n])$；对于有边直接连接的两点$x$和$y$，$d[k][x][y] = 边长$。

转移方程：$f[k][x][y] = min\{f[k - 1][x][y], f[k - 1][x][k] + f[k - 1][k][y]\}$

考虑状态压缩，显然$f[k][x][k]$是一定等于$f[k - 1][x][k]$，因为$x$到$k$的路径不可能以点$k$本身为中转节点；同理，$f[k][k][y] = f[k - 1][k][y]$。

于是，我们可以直接压缩掉第一维（$k$），新的状态为$d[x][y]$（$x$和$y$两点的最短路径长度），转移方程为$f[x][y] = min\{f[x][y], f[x][k] + f[k][y]\}$

**代码实现：**

```cpp
for(int k = 1; k <= n; k++) {
	for(int i = 1; i <= n; i++) {
		for(int j = 1; j <= n; j++) {
			d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
		}
	}
}
```

Floyd的思想其实就是“通过逐步引入新的中继节点，来计算对应节点/状态间的最优路径”。在标准的Floyd算法中，**“最优路径”**指的就是**最短路**，但实际上，Floyd算法还可以解决一些其他的问题.

比如[这道题（洛谷P2888）](https://www.luogu.com.cn/problem/P2888)，我们根据Floyd的基本思想，就可以设计出转移方程$f[x][y] = min\{f[x][y], max\{f[x][k] + f[k][y]\}\}$

具体实现（其实就只改了转移方程）：

```cpp
for(int k = 1; k <= n; k++) {
	for(int i = 1; i <= n; i++) {
		for(int j = 1; j <= n; j++) {
			d[i][j] = min(d[i][j], max(d[i][k], d[k][j]));
		}
	}
}
```

## Updated on 2022/8/7

#### 关于Floyd思想的另一种应用

其实Floyd还可以处理支持[传递闭包](https://blog.csdn.net/qq_41280600/article/details/104128144)的问题。

集体实现：

```cpp
for(int k = 1; k <= n; k++) {
	for(int i = 1; i <= n; i++) {
		for(int j = 1; j <= n; j++) {
			d[i][j] |= d[i][k] & d[k][j];
			// 只要d[i][k]和d[k][j]都能满足，那么d[i][j]也能满足
		}
	}
}
```

直接上例子：[CF500B New Year Permutation](https://www.luogu.com.cn/problem/CF500B)

这道题中，数组中「元素的交换」就支持**传递闭包**，即：若a和b可以交换，b和c也可以交换，那么a和c就一定可以通过b来间接交换。所以，我们也可以使用Floyd算法来解决。

## Updated on 2022/8/10

再附上**Floyd求最小环**的模板：

```cpp
int ans = 0x1f1f1f1f;
for(int k = 1; k <= n; k++) {
	for(int i = 1; i <= n; i++) {
		for(int j = i + 1; j <= n; j++) {
			ans = min(ans, d[i][j] + w[i][k] + w[k][j]);
		}
	}
	for(int i = 1; i <= n; i++) {
		for(int j = 1; j <= n; j++) {
			d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
		}
	}
}
```
