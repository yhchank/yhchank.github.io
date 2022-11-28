---
title: P4715 淘汰赛 - 题解
date: 2022-07-25T16:02:53+08:00
tags: [C++/算法]
---
# P4715 淘汰赛

### 题目描述

有 2^n (n≤7) 个国家参加世界杯决赛圈且进入淘汰赛环节。我经知道各个国家的能力值，且都不相等。能力值高的国家和能力值低的国家踢比赛时高者获胜。1 号国家和 2 号国家踢一场比赛，胜者晋级。3 号国家和 4 号国家也踢一场，胜者晋级……晋级后的国家用相同的方法继续完成赛程，直到决出冠军。给出各个国家的能力值，请问亚军是哪个国家？

### 工具

队列，二叉树

### 代码实现

```c++
#include <iostream>
#include <queue>
using namespace std;

int n;
int num[130] = {};
queue<int> ids;

int main() {
	cin >> n;
	for(int i = 1; i <= (2 << (n - 1)); i++) {
		cin >> num[i];
		ids.push(i);
	}
	while(ids.size() > 2) {
		int a = ids.front();
		ids.pop();
		int b = ids.front();
		ids.pop();
		int maxid = ((num[a] > num[b]) ? a : b);
		ids.push(maxid);
	}
	int a = ids.front();
	ids.pop();
	int b = ids.front();
	ids.pop();
	cout << ((num[a] < num[b]) ? a : b);
	return 0;
}

```
