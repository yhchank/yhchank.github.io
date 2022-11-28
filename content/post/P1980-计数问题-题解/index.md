---
title: P1980 计数问题 - 题解
date: 2022-07-25T16:03:29+08:00
tags: [C++/算法]
---
# P1980 计数问题

## 题目描述

试计算在区间 1 到 n的所有整数中，数字x(0 ≤ x ≤ 9)共出现了多少次？例如，在 1到11中，即在 1,2,3,4,5,6,7,8,9,10,11中，数字1出现了4次。

## 输入格式

2个整数n,x，之间用一个空格隔开。

## 输出格式

1个整数，表示x出现的次数。

## 输入输出样例

**输入 #1**

```
11 1
```

**输出 #1**

```
4
```

## 题解

string to_string (int val);

string to_string (double val);

这是C++11新增的库函数，已重载各种标准类型，用法很简单，例如 string s = to_string(123);

但是对于大量数据只能一个数一个数转换，颇为繁琐

还有一种方法就是 stringstream (使用stringstream 需包含头文件 <sstream>）

int a = 123, b = 456, c = 789;

stringstream ss;

ss << a << b << c;

以上几行把a b c三个变量读入字符串流中，这样就可以把a b c三个变量无缝衔接起来，但是需要注意的是，stringstream型不能直接输出，也不支持迭代器，需要利用stringstream类的成员函数str()才能把它当成普通字符串来用，例如

string s = ss.str();

而把字符串转为数值的话也是用stringstream，例如上面的ss就可以转换为一个int型

int i;

ss >> i;

以上都是C++的方法，c语言有sprintf和sscanf，对应stringstream字符串流的读和写，<stdlib.h>中还有数值与字符串相互转换的iota和atoi函数，感兴趣的可以自己去了解。用c语言的sprintf比C++的字符串流效率高，但是我还是想用C++的字符串，为啥？因为C++字符串的可拓展性和灵活性比较高嘛hhhh

------

count函数，位于 <algorithm> 头文件中

count(ivec.begin() , ivec.end() , searchValue)

这个函数作用是统计在一定范围内某一值出现的次数

前两个参数为起始位置和结束位置，都是迭代器，第三个参数为需要统计的值

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
   int n, x;
   cin >> n >> x;
   stringstream ss;
   for(; n >= 1; n--) {
       ss << n;
   }
   string s = ss.str();
   cout << count(s.begin(), s.end(), x + '0');
   return 0;
}
```
