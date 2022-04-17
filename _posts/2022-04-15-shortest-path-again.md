---
title: 图中的最短路径
author: saltycat
date: 2022-04-15 11:13:51 +0800
categories: [图论,算法]
tags: [图论,最短路]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io

---

# 单源最短路

## Ford 算法

（to be updated）

## Dijkstra 算法

定义一个集合 $S =\{源点s + 已经确定了最短路径的 V\}$（此处的确定最短路径是指，只用集合中收录了的点确定了最短路，也就是说每收录一个新的点，就有可能改变这个最短路径），定义数组 $dist[i]$ 表示 $s$ 到 $i$ 只经过集合 $S$ 中顶点的最短路径距离。

怎么收录 $V$ 呢？我们每一次搜寻 $dist[i]$ 最小的且未收录的点到集合 $S$ 中（贪心）。由于收录的点可能与集合 $S$ 中的点存在边，也就是收录的点 $V$ 可能会改变其他点 $u$ 的最小路径 $dist[u]$（可能通过 $V$ 到 $u$ 距离会变短）。

如图，未收录 $V$ （灰色表示）时，这个时候 $dist[u]=22$。

![未收录V时](/assets/blog_res/2022-04-15-shortest-path-again.assets/image-20220415174901468.png)

若将 $V$ 收录后，则需要更新更新，集合中其他顶点的 $dist[]$ 值：

为了维护集合中的点最短路是最小值，每一次收录一个顶点 $V$，对 $\forall{u}\in S $ ，$dist[u]=\min(dist[u],dist[v]+e<v,u>)$ （$e<v,u>$ 为边 $vu$ 的权重）

如图，由于 $dist[V]+e<V,U>=6+4=10<d[U]=22$，故更新 $dist[U]=10$

![收录V更新后](/assets/blog_res/2022-04-15-shortest-path-again.assets/image-20220415175444054.png)

如图可知，此时收录至集合中的顶点（橙色）的 $dist$ 值都是最小的了。

因此只要不断将新的点收录至集合中，就可以得到每个点到源点的最小距离。

### 直接扫描未收录顶点

```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#define MAXN 10000
#define INF 9999999
using namespace std;
int edge[MAXN][MAXN];
// dist[]表示从原点到i的最小距离，abandoned[]表示是否访问过
// path[]表示前驱点，用于输出路径 
int dist[MAXN],path[MAXN],abandoned[MAXN];
int m,n;

void all_reset(int n){
    // 不要用 memset()初始化为INF 
    for(int i=1;i<=n;++i)
        for(int j=1;j<=n;++j)
            edge[i][j]=(i==j)?0:INF;
    return;
}
void reset(int n){
    for(int i=1;i<=n;++i){
        dist[i]=INF;
        abandoned[i]=0;
        path[i]=-1;
    }
    return;
}

void Dijkstra(int s){
    // 初始化 
    reset(n);
    dist[s]=0;
    for(int i=1;i<=n;++i){ // 外层循环用来遍历所有点 
        int min=INF;
        int w;
        for(int j=1;j<=n;++j) // 贪心部分，寻找最短距离的点（向外扩张） 
            if(!abandoned[j] && dist[j]<min){
                min=dist[j];
                w=j;
            }
        // 找到了，标记一下 
        abandoned[w]=1;
        for(int v=1;v<=n;++v){ // 更新集合内其他点的最小值 
            if(dist[v]>dist[w]+edge[w][v]){
                dist[v]=dist[w]+edge[w][v];
                path[v]=w;
        	}
    	}
    return;
}

int main(){
    int u,v,d;
    scanf("%d%d",&n,&m);

    // 初始化边 
    all_reset(n);

    // 读入边  
    for(int i=1;i<=m;++i){
        scanf("%d%d%d",&u,&v,&d);
        edge[u][v]=d;
    }
    
    Dijkstra(1);
    
    // 打印dist[]数组 
    for(int i=1;i<=n;++i){
        printf("dist[%d]=%d\n",i,dist[i]);
    }
    return 0;
}
```



