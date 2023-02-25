---
title: "在C++中检测算术溢出"
date: 2023-02-25T11:50:22+08:00
draft: true
---

在编写代码的过程中，算术溢出是一件让人十分头疼的事。因此在代码中检测是否溢出是一件很有必要的事

我们可以利用以下函数检测 `a * b` 是否有溢出：

```cpp
bool overflow(int a, int b) {
    if (b == 0) {
        return 0;
    }
    int tmp = a * b;
    tmp /= b;
    return tmp == a ? 0 : 1;
}
```

这样，在有可能发生溢出的乘法计算（假设为 `x * y`）前添加 `assert(!overflow(x, y))` 即可检测
