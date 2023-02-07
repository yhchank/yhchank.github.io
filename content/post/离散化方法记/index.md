---
title: "离散化方法记"
date: 2023-02-06T09:26:40+08:00
draft: false
---

**离散化的作用**：数值压缩

# 方法一：排序后确定排位

**注**：

1. `id[]` 表示第 $i$ 大的数在原数组中的编号
2. `rk[]` 表示原数组中编号为 $i$ 的数的排位（第几大）

## 实现

```cpp
bool cmp(const int a, const int b) {
    return x[a] < x[b];
}

int main() {
    // ...
    for (int i = 1; i <= n; i++) {
        cin >> x[i];
    }
    for (int i = 1; i <= n; i++) {
        id[i] = i;
    }
    sort(id + 1, id + n + 1, cmp);
    for (int i = 1; i <= n; i++) {
        rk[id[i]] = i;
    }
    // ...
}
```

# 方法二：副本排序+去重+二分定位

## 实现

```cpp
int main() {
    // ...
    for (int i = 1; i <= n; i++) {
        cin >> x[i];
        y[i] = x[i];
    }
    sort(y + 1, y + n + 1);
    int nU = unique(y + 1, y + n + 1) - y - 1;
    for (int i = 1; i <= n; i++) {
        rk[i] = lower_bound(y + 1, y + nU + 1, x[i]) - y;
    }
    // ...
}
```

