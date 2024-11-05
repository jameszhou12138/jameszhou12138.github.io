---
title: "Algorithm Template"
collection: algorithms
category: templates
permalink: /algorithm/template
excerpt: ''
---
<!-- 熟悉markdown都知道可以使用[TOC]自动生成markdown文件的标题目录，比如在typora，vscode(需要插件)等本地编辑器中，或者在CSDN等网页编辑器中，但是github却不支持[TOC]标签。

笔者目前了解到的最最最简单的莫过于VSCode中的Markdown All in One 插件了，安装后点开md文件，然后快捷键CTRL(CMD)+SHIFT+P，输入Markdown All in One: Create Table of Contents回车即可 -->

- [1. 时间复杂度](#1-时间复杂度)
- [2. 基础算法](#2-基础算法)
  - [2.1 快速排序](#21-快速排序)
  - [2.2 归并排序](#22-归并排序)
  - [2.3 二分](#23-二分)
    - [2.3.1 整数二分](#231-整数二分)
    - [2.3.2 浮点数二分](#232-浮点数二分)
  - [2.4 前缀和](#24-前缀和)
    - [2.4.1 一维前缀和](#241-一维前缀和)
    - [2.4.2 二维前缀和](#242-二维前缀和)
  - [2.5 差分](#25-差分)
    - [2.5.1 一维差分](#251-一维差分)
    - [2.5.2 二维差分](#252-二维差分)
- [3 kuangbin的模板](#3-kuangbin的模板)

## 1. 时间复杂度

> C++中1s的操作次数控制在 $10^7$ ~ $10^8$。不同数据范围下，代码的时间复杂度和算法的选择如下表所示：

| n | 时间复杂度 | 算法 |
| :--: | :--: | -- |
| $30$ | 指数级别 | dfs+剪枝、状态压缩dp |
| $100$ | $O(n^2)$ | floyd、dp、高斯消元 |
| $1000$ | $O(n^2)，O(n^2\log{n})$ | dp、二分、朴素版Dijkstra、朴素版Prim、Bellman-Ford |
| $10^4$ | $O(n\sqrt{n})$ | 块状链表、分块、莫队 |
| $10^5$ | $O(n\log{n})$ | 各种sort、线段树、树状数组、setmap、heap、拓扑排序、dijkstra+heap、prim+heap、Kruskal、spfa、求凸包、求半平面交、二分、CDQ分治、整体二分、后缀数组、树链剖分、动态树 |
| $10^6$ | $O(n)$ | 单调队列、hash、双指针扫描、BFS、并查集、kmp、AC自动机 |
| $10^6$ | 常数较小的 $O(n\log{n})$ | sort、树状数组、heap、dijkstra、spfa |
| $10^7$ | $O(n)$ | 双指针扫描、kmp、AC自动机、线性筛素数 |
| $10^9$ | $O(\sqrt{n})$ | 判断质数 |
| $10^{18}$ | $O(\log{n})$ | 最大公约数、快速幕、数位DP |
| $10^{1000}$ | $O((\log{n})^2)$ | 高精度加减乘除 |
| $10^{100000}$ | $O(\log{k} \times \log{\log{k}})$，$k$表示位数 | 高精度加减、FFT/NTT |



## 2. 基础算法

### 2.1 快速排序

```c++
void quick_sort(int q[], int l, int r) {
    if (l >= r) {
        return ;
    }
    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j) {
        do i++; while (q[i] < x);
        do j--; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, i);
    quick_sort(q, j + 1, r);
}
```

### 2.2 归并排序

```c++
void merge_sort(int q[], int l, int r) {
    if (l >= r) return ;
    int mid = l + r >> 1;
    merge_sort(q, l, mid), merge_sort(q, mid + 1, r);
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) tmp[k++] = q[i++];
        else tmp[k++] = q[j++];
    while (i <= mid) tmp[k++] = q[i++];
    while (j <= r) tmp[k++] = q[j++];
    for (i = l, j = 0; i <= r; i++, j++) q[i] = tmp[j];
}
```

### 2.3 二分

#### 2.3.1 整数二分

```c++
// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r) {
    while (l < r) {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r) {
    while (l < r) {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

#### 2.3.2 浮点数二分

```c++
double bsearch_3(double l, double r) {
    for (int i = 0; i < 100; i++) {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```

### 2.4 前缀和

#### 2.4.1 一维前缀和

```c++
sum[i] = a[i] + a[2] + ... + a[i]
a[l] + ... + a[r] = sum[r] - sum[l - 1]
```

#### 2.4.2 二维前缀和

```c++
sum[i, j] = 第i行j列格子左上部分所有元素的和
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：sum[x2, y2] - sum[x1 - 1, y2] - sum[x2, y1 - 1] + sum[x1 - 1, y1 - 1]
```

### 2.5 差分

#### 2.5.1 一维差分
```c++
给区间[l, r]中的每个数加上c：ca[l] += c, ca[r + 1] -= c
```

#### 2.5.2 二维差分
```c++
给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：
ca[x1, y1] += c, ca[x2 + 1, y1] -= c, ca[x1, y2 + 1] -= c, ca[x2 + 1, y2 + 1] += c
```


## 3 kuangbin的模板
<iframe src="https://jameszhou12138.github.io/files/kuangbin的ACM模板.pdf" width="800" height="800"></iframe>
