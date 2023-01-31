---
title: "算法思维导图"
date: 2023-01-31T10:11:02+08:00
mindmap: true
menu:
    main:
        weight: -60
        params: 
            icon: infinity
---

- “满足要求的最大值”“最大值最小” => 考虑**二分**
- “字典序最小/最大的结果” => 考虑贪心（因为使当前项更小一定会使结果更优），如[CF1547D Co-growing Sequence](https://www.luogu.com.cn/problem/CF1547D)就是一道按位贪心的题目（实际解法可以直接用位运算而不是按位枚举）；**补充：** 如果在某些涉及位运算及相关知识的题目中，可能会出现“最大值”“最小值”等问题，此时也可以考虑从高到低按位贪心（类似于字典序最小），例题：[CF1554C Mikasa](https://www.luogu.com.cn/problem/CF1554C)
- 对点集排序，某些点需满足特定关系 => 考虑**拓扑排序**
- 遇到DP题，先从简单情况入手（有时就是DP的初始状态）。比如[CF187B AlgoRace](https://www.luogu.com.cn/problem/CF187B)，这道题就可以先考虑不换车的情况（Floyd板子），再列转移方程，推广到其他情况
- 遇到判**正环**或者求**最长路**，将所有的边权**取相反数**，例题：[P1938 \[USACO09NOV\]Job Hunt S](https://www.luogu.com.cn/problem/P1938)
- 没有思路先暴力，考虑去除冗余情况有时也是一种解法
- 求区间$(l, r)$的个数或找所有满足要求的区间 => 有时可以考虑双指针（尺取法）
- 当整体问题不便于解决时，可以考虑每个“个体”对整体的贡献，例1：[CF547B Mike and Feet](https://www.luogu.com.cn/problem/CF547B)，此题中则可以考虑每个值可以作为那些区间的最小值（单调栈解决）；例2：[P2422 良好的感觉](https://www.luogu.com.cn/problem/P2422)，此题中则可考虑每一天为最不舒服的那一天时，区间的感受值最大为多少（区间尽量大，仍然是单调栈解决）
- 若区间左端点 $\in [l, mid]$，区间右端点 $\in [mid, r]$（即 $mid$ 一定在区间中），则最大区间和等于 $\max_{i=mid}^{r}sum_i - \min_{i=l-1}^{mid-1}sum_i$，其中 $sum_i$ 表示前缀和
- 对于**修改+查询**类题目，若有两种解法，一种修改 $O(n)$、查询 $O(1)$，一种修改 $O(1)$、查询 $O(n)$，那么可以考虑利用分块进行折中；典型的分块优化方式为**分成两层处理**：修改/查询时 $O(\sqrt{n})$ 处理整块部分（块层次），再 $O(\sqrt{n})$ 处理区间两端的零碎部分（个体层次）；例题：
  - [[太戈 961] 最强大脑之十二](https://www.luogu.com.cn/problem/U277750)，[代码](https://www.luogu.com.cn/paste/z9oqwgjn)
  - [[太戈 1058] 进攻长城](https://www.luogu.com.cn/problem/U277755)，[代码](https://www.luogu.com.cn/paste/wd605oxt)
  - [[太戈 1060] 提前交卷](https://www.luogu.com.cn/problem/U277718)，[代码](https://www.luogu.com.cn/paste/4ws3mlmj)
- “区间 $[l, r]$ 内小于等于 $k$ 的元素的个数”
  - 静态：区间排序+二分（`lower_bound()` / `upper_bound()`），例：[[太戈 1058] 进攻长城](https://www.luogu.com.cn/problem/U277755)，[代码](https://www.luogu.com.cn/paste/wd605oxt)
  - 动态：树状数组维护
    1. 当添加元素 $a[i]$ 时，`add(a[i], 1)`
    2. 对 $1$ 至 $l - 1$ 号元素 `add(a[i], 1)`，此时 `query(k)` 就是区间 $[1, l - 1]$ 中小于等于 $k$ 的元素个数
    3. 继续对 $l$ 至 $r$ 号元素 `add(a[i], 1)`，此时 `query(k)` 就是区间 $[1, r]$ 中小于等于 $k$ 的元素个数
    4. 两者相减