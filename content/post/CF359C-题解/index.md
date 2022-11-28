---
title: CF359C 题解
date: 2022-08-17T10:36:30+08:00
tags: [C++/算法]
---

## 题面 

[点这里](https://www.luogu.com.cn/problem/CF359C) 

## 分析 

可以先回忆一下我们平时是如何通分的（以*样例#2*为例）。 

首先，把分母全部乘起来： 

$$\frac{1}{3^1}+\frac{1}{3^2}+\frac{1}{3^3}=\frac{3^2 \times 3^3+3^1 \times 3^3+3^1 \times 3^2}{3^1 \times 3^2 \times 3^3}$$ 

把指数加起来，得到： 

$$\frac{3^5+3^4+3^3}{3^6}$$ 

显然，分子和分母上的任意一个数都是 $3^n$，也就是说：我们只需要找到分子中**次数最低的一项**（$3^3$）去跟分母（$3^6$）比较，输出其中次数更低的一项（$3^3$）即可。 

稍作观察即可得到答案的公式：$ans = \min(sum - a_i)$（其中 $sum$ 表示所有 $a_i$ 的和）。 

**但是**，看到样例一，这里出现了两个 $2$，根据 $ans = \min(sum - a_i)$，此时的答案应为 $2^2=4$，但是实际答案为 $8$。 

为什么？显然是因为 $2^2+2^2=2 \times 2^2=2^3$（只看分母），也就是说，当 $a_i$ 有重复时，要把每个值的个数也考虑进去，比如说，$12$ 个 $2^2$ 就应该写成 $3 \times 2^4$。 

所以我们可以统计每个 ${sum - a_i}$ 的值的出现次数，然后计算**出现次数**中有几个因数 $x$，可以用这样一个函数来实现： 

```cpp
int count(int a, int &b) { // 计算b中有几个因数a，用“&b”是因为处理完后b剩余的数还要用到
	int res = 0;
	while (b && b % a == 0) { // 如果b大于0且还有因数a
		res++; // 个数加一
		b /= a; // 除掉因数a
	}
	return res;
}
```

但是，如果你这样写，还是会 `WA`，因为有可能出现这样的情况： $2^2 \times 8 + 2^3 \times 4 = 2^2 \times 2^3 + 2^3 \times 2^2 = 2^5 + 2^5 = 2 ^ 6$。也就是说，即使考虑了每个 ${sum - a_i}$ 的值的出现次数，在计算的过程中，还是有可能出现**重复的情况**。 

对于这个问题，我的解决办法： 

首先，开一个 `Number` 类型（定义见下方）的优先队列。 

```cpp
struct Number {
	int k; // x的k次方
	int cnt; // 有cnt个
	bool operator>(const Number &_x) const { // 重载大于运算符，优先队列中会用到
		return k > _x.k;
	}
};


priority_queue<Number, vector<Number>, greater<Number>> q;
```

在输入完成后，先预处理出每个 $sum - a_i$，放在 $b$ 数组中，代码如下： 

```cpp
int sum = 0;
for (int i = 1; i <= n; i++) { // 计算a[i]的和
	sum += a[i];
}
for (int i = 1; i <= n; i++) { // 预处理出每个sum - a[i]的值
	b[i] = sum - a[i];
}
```

然后，统计每个 $b_i$ 值的个数，一起 `push` 到优先队列里： 

```cpp
int tmp = 1;
b[0] = -1;
for (int i = n - 1; i >= 0; i--) { // 倒着统计（其实正向也可以）
	if (b[i] != b[i + 1]) {
		// 把sum - a[i]（即b[i]）的值和它的个数一起push到单调队列里
		q.push({b[i + 1], tmp});
		tmp = 0; // 清除计数器
	}
	tmp++; // 计数器加一
}
```

每次取出次数最小的数并不断检查剩下的队首元素，如果次数和当前元素的次数相同，那么就合并 `cnt` 最后，调用上文提到的 `count()` 函数来处理 *$b_i$值的个数*，并把结果乘到当前元素上，把 `count()` 处理完的 `cnt` （也就是不能再拆出 $x$ 了）最为新的 `cnt`，然后重新加入队列；**注意：** 如果 `count()` 函数的返回值为 $0$ ，则意味着 `cnt` 中已经拆不出来因数 $x$ 了，所以此时的答案就是当前元素的次数，可以直接 `break`，代码： 

```cpp
while (!q.empty()) {
	Number f = q.top(); // 每次取出次数最小的数
	q.pop();
	while (!q.empty() && q.top().k == f.k) { // 如果队首元素的次数和当前元素的次数相同
		f.cnt += q.top().cnt; // 合并个数
		q.pop();
	}
	ans = f.k; // 更新答案
	int tmp = count(x, f.cnt); // 计算cnt中能拆出开的因数x的个数
	if (tmp) {
		// k加上处理结果，剩下拆不掉的数仍然作为cnt
		q.push({f.k + tmp, f.cnt});
	} else {
		// cnt中拆不出来因数x了，可以直接结束
		break;
	}
}
```

由于分子的结果可能比分母的次数高，还需要取一下最小值： 

```cpp
ans = min(ans, sum);
```

最后用快速幂算出 $x^{ans}$ 即可。 

## 完整代码

最然看起来很长，其实内容并不多。 

```cpp
/*
*	Author: Hank Yu
*	Problem: CF359C
*	Date: 2022/08/16
*/

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"

void faster_io() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
}

#define int long long

#define MAXN 100005
#define MOD 1000000007

struct Number {
	int k; // x的k次方
	int cnt; // 有cnt个
	bool operator>(const Number &_x) const { // 重载大于运算符，优先队列中会用到
		return k > _x.k;
	}
};

int n, x;
int a[MAXN];
int b[MAXN];
priority_queue<Number, vector<Number>, greater<Number>> q;

int count(int a, int &b) { // 计算b中有几个因数a
	int res = 0;
	while (b && b % a == 0) { // 如果b大于0且还有因数a
		res++; // 个数加一
		b /= a; // 除掉因数a
	}
	return res;
}

int qpow(int x, int k, int q) { // 快速幂模板
	int ans = 1;
	while (k > 1) {
		if (k % 2 == 1) {
			ans *= x;
			ans %= q;
		}
		x *= x;
		x %= q;
		k /= 2;
	}
	if (k == 1) {
		ans *= x;
		ans %= q;
	}
	return ans;
}

signed main() {
	faster_io(); // cin/cout加速
	cin >> n >> x;
	for (int i = 1; i <= n; i++) { // 输入
		cin >> a[i];
	}
	int sum = 0;
	for (int i = 1; i <= n; i++) { // 计算a[i]的和
		sum += a[i];
	}
	for (int i = 1; i <= n; i++) { // 预处理出每个sum - a[i]的值
		b[i] = sum - a[i];
	}
	{ // 防止变量名污染
		int tmp = 1;
		b[0] = -1;
		for (int i = n - 1; i >= 0; i--) { // 倒着统计（其实正向也可以）
			if (b[i] != b[i + 1]) {
				// 把sum - a[i]（即b[i]）的值和它的个数一起push到单调队列里
				q.push({b[i + 1], tmp});
				tmp = 0; // 清除计数器
			}
			tmp++; // 计数器加一
		}
	}
	int ans = 0;
	{ // 防止变量名污染
		while (!q.empty()) {
			Number f = q.top(); // 每次取出次数最小的数
			q.pop();
			while (!q.empty() && q.top().k == f.k) { // 如果队首元素的次数和当前元素的次数相同
				f.cnt += q.top().cnt; // 合并个数
				q.pop();
			}
			ans = f.k; // 更新答案
			int tmp = count(x, f.cnt); // 计算cnt中能拆出开的因数x的个数
			if (tmp) {
				// k加上处理结果，剩下拆不掉的数仍然作为cnt
				q.push({f.k + tmp, f.cnt});
			} else {
				// cnt中拆不出来因数x了，可以直接结束
				break;
			}
		}
	}
	ans = min(ans, sum); // 由于分子的结果可能比分母的次数高，这里需要取最小值
	cout << qpow(x, ans, MOD) << endl;
}
```

## AC
