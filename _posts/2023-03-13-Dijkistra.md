---
layout: post
toc: true
title: "Dijkistra Optimize"
categories: OI
tags: [OI,图]
author: 
  - Tingyu Song 
---

一种经典的优化Dijkistra算法的方法。

### 1 DataStructure

链式前向星。

当采用邻接矩阵存储图时，dijkistra的时间复杂度是$O(N^2)$，而当采用邻接表或者链式前向星时，时间复杂度是$O((N+E)logN)$。所以通常来说我们会选择邻接矩阵存储边，如

```c++
vector<vector<int>> v;
```

对于边`1 2`，也就是有一条从`1`到`2`的边，我们可以

```
v[1].push_back(2)
```

或者当边有权重时，我们可以

```c++
vector<vector<pair<int,int>> v(n); // n is the node numbers
v.emplace_back(2,3) // 2 is the dst, and 3 is weight
```

但是因为vector的自动扩容机制，每次1.5倍扩容的话，导致部分题目不通过。因而选择链式向前星作为数据结构。

```c++
const int maxn = 1000000;
int cnt = 0; // cnt是edge的索引
struct Edge {
  int to;
  int next;
  int w;
} edge[maxn];
// head里记录的是cnt，比如head[1]=3,表示从1出发的点中，有1条边，这条边的索引为3
// 而如果从1出发的不只一条边，那么可以从edge[cnt]->next中找到下一条边的索引。
int head[maxn];
```

添加1条边的函数为

```c++
void addedge(int u,int v,int w) {
  cnt++; 
  edge[cnt].to = v;
  edge[cnt].w = w;
  edge[cnt].next = head[u]; // 总是指向之前的那条边
  // 把以u为首的边的集合 的最新的 边，更新为当前的边
  // 相当于每次向前找
  head[u] = cnt; 
}
```

遍历 以某点`u`开始的边集的方法为，

```c++
for(int i = head[u]; head[i] != 0; i = head[u].next) {
  // some code here
}
```

### 2 Dijkistra with Heap

在传统的dijkistra中，每次选择距离起点最近的顶点需要$O(N)$时间，这样每次都需要耗费很久的时间。可以采用堆的方式进行优化。

与上面的遍历的代码结合就是如下

```c++
// 注意这里的priority queue对pair进行排序时，首先会对first进行排序，之后对second的进行排序
priority_queue<pair<int,int>, vector<pair<int,int> >, greater<pair<int,int> > > q;
for(int i = head[u]; head[i] != 0; i = head[u].next) {
  	if(dis[edge[i].to] > dis[u]+edge[i].w) {
      dis[edge[i].to] = dis[u] + edge[i].w;
      q.emplace_back({dis[edge[i].to],i});
    }
}
```

### 3 练习

[P3371 单源最短路径](https://www.luogu.com.cn/problem/P3371)



