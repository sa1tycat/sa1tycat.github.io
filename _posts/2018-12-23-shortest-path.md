---
title: 图中的最短路径
author: saltycat
date: 2018-12-23 14:47:16 +0800
categories: [图论,算法]
tags: [图论]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io

---

# Floyd算法

可求多源最短路径。

## 基本思想

 - DP
 - 设 $d[i][j][k]$ 表示从 $i$ 到 $j$ 的路径中，经过的点的编号不超过 $k$ 的最短路。
 - 边界条件 $d[i][j][0] = dis[i][j],d[i][i]=0$, 余下 $d[i][j]= \mathrm{INF}$;
 - 转移方程:

$$d[i][j][k] = \min(d[i][j][k-1],d[i][k][k-1]+d[k][j][k-1]) (0<k,0\leqslant i,j\leqslant n)$$

则 $dp[i][j][n]$ 即为所求

## 代码

$\mathcal{O}(n^3)$

```c++
for(k=1;k<=n;++k)
    for(i=1;i<=n;++i)
        for(j=1;j<=n;++j)
            if(d[i][j]>d[i][k]+d[k][j])
                d[i][j]=d[i][k]+d[k][j];
```

# Dijsktra算法

## 松弛

松弛操作
`D[i]`表示源点`s`到`i`的当前最短路径

1. 条件：`d[i]+e[i][j]<d[j]`

2. 更新：`d[j]=d[i]+e[i][j]`

## 基本思想

 - 这是一个贪心算法。
 - 算法初始时d[s]=0，其余的点的d[]值为INF。有S、T两个集合，S集合中包含所有已求得最短路的点，T集合包含未求得最短路的点。
 - 当T集合非空时，从T集合中选取d[]值最小的一个点，如果该点的d[] !=INF，则把它放入S集合，此时d[]值就是该点的最短路值。

 - 当T集合为空或从T中选出的点的d[]==INF时，算法结束。

## 朴素版伪代码

复杂度 $\mathcal{O}(n^2)$

```c++
void dijkstra(int s,int t) {
    初始化S={空集}, T=全集-S;
    d[s] = 0; 其余d值为INF
    while (T非空 && T中最小的d[] != INF)
    {
        取出T中具有最小d[]的点i;
        for (所有不在S中且与i相邻的点j)
          if (d[j] > d[i] + cost[i][j]) d[j] = d[i] + cost[i][j]; ( “松弛”操作” )
        S = S + {i}; //把i点添加到集合S里
        T = T – {i};
    }
    return;
}
```

### 说明

 - 每次选取的`j`使`d[]`最小，这样可以保证它的路径已经是最短路。因为如果经过某个未标记点`p`到达`j`会产生更短的路，那么到达`j`的最短路长度必然要加上一个更大的`d[p]`，矛盾

 - `d[k]=min{d[k],d[j]+cost[j][k]}`的作用是在标记j以后更新`d[]`数组（松弛操作）
 - 每次循环会对`1`个点进行标记，所以`n-1`次循环后所有点都做标记，`d[]`的含义变成可以经过所有点的最短距离，就是我们要的最短距离
 - 如果找到的`d[]`最小的一个是`d[]=INF`，则可以提前结束循环，未做标记的点都是不可到达的
 - 不能处理负权值边

##朴素版代码(邻接矩阵)

``` c++
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

void Dijsktra(int s){
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
        for(int v=1;v<=n;++v){ // 寻找下一条边 
            if(dist[v]>dist[w]+edge[w][v])
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
    
    
    Dijsktra(1);
    
    // 打印dist[]数组 
    for(int i=1;i<=n;++i){
        printf("dist[%d]=%d\n",i,dist[i]);
    }
    return 0;
}
```
## 优先队列优化

使用结构体的成员函数和运算符重载以及优先队列，复杂度 $\mathcal{O}(n\lg n)$

**结构体的定义：**

```c++
struct node{
    int u,d;
    node(){};
    node(int a, int b){
        u=a;d=b;
    }
    bool operator < (const node& a)const{
        return d>a.d;
    }
};
```

**代码解释：**

 - 赋值更简单
赋值可以直接写为`node t(1,2)`，等价于`t.u=1;t.d=2`
 - 适配了优先队列
 因为定义了<，使得优先队列可以运行



## 优先队列+邻接矩阵代码

```c++
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
#include<queue>
#define MAXN 10001
#define INF 2e8
using namespace std;
int g[MAXN][MAXN],d[MAXN],path[MAXN];
bool abandoned[MAXN];
int n,m;
struct node{
    int u,d;  // 源点到u的最短路径d 
    node(){};
    node(int a, int b){  //赋值函数，用法:node 变量名(参数1,参数2) 
        u=a;d=b;
    }
    bool operator < (const node& a)const{ //重载< 
        return d>a.d;
    }
};
void Dijsktra(int s){
    priority_queue <node> q;  // 定义一个node型的优先队列 
    /*======初始化======*/
    q.push(node(s,0));
    memset(abandoned,0,sizeof(abandoned));
    memset(path,-1,sizeof(path));
    for(int i=1;i<=n;++i){
        d[i]=INF;
    }
    d[s]=0;
    while(!q.empty()){
        node x=q.top();
        q.pop();
        int u=x.u;
        if(abandoned[u]) continue;  // 找到最短路径，因为队列中有重复元素
        abandoned[u]=1;
        for(int v=1;v<=n;++v){
            if(!abandoned[v] && g[u][v]<INF){  //找相邻的节点 
                if(d[v]>d[u]+g[u][v]){  //松弛操作 
                    d[v]=d[u]+g[u][v];
                    q.push(node(v,d[v]));
                }
            }
        }
        
    }
    return; 
}

int main(){
    int u,v,w,s;
    scanf("%d%d",&n,&m);
    /*=======初始化=======*/
    for(int i=1;i<=n;++i)
        for(int j=1;j<=n;++j)
            g[i][j]=(i==j)?0:INF;
    /*=======读入=========*/
    for(int i=1;i<=m;++i){
        scanf("%d%d%d",&u,&v,&w);
        g[u][v]=w;
    }
    scanf("%d",&s);
    Dijsktra(s);
    /*======输出到所有结点的最短路径长度======*/
    for(int i=1;i<=n;++i){
        printf("%d -> %d: ",s,i);
        if(d[i]==INF){
            printf("INF\n");
        }
        else{
            printf("%d\n",d[i]);
        }
    }
    return 0;
}
```

# SPFA

Ford算法的优化，时间复杂度小于等于 $\mathcal{O}(NE)$（吧？？）

## 伪代码：
```C++
void SPFA(int s){
    //初始化
    d[]=INF;
    pushed[]=0;
    d[s]=0;
    q.push(s);
    pushed[s]=1;
    
    while(!q.empty()){
        u=q.front();
        q.pop();
        pushed[u]=0;
        for(v=起点为u的边的终点){  // 注意穷举的是边
            if(d[edge[v].v]>edge[v].w+d[u]){
                d[edge[v].v]=edge[v].w+d[u];
                q.push(edge[v].v);
            }
        }
    }
}
```

## 具体应用
[一本通P1382](http://ybt.ssoier.cn:8088/problem_show.php?pid=1382)

```C++
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<cmath>
#include<queue>
#define INF 1e7
#define MAXN 1000010
using namespace std;
queue <int> q;
int n,m;
struct myType{
    int u,v;
    int w;
    int next;
}edge[MAXN];
int head[MAXN];
int k;
void add(int u,int v,int w){
    k++;
    edge[k].next=head[u];
    head[u]=k;
    edge[k].u=u;edge[k].v=v;edge[k].w=w;
    return;
}
int d[MAXN];
int path[MAXN];
bool pushed[MAXN];
void SPFA(){
    /* =======初始化======= */
    for(int i=1;i<=n;++i)
        d[i]=INF;
    d[1]=0; //源点为0;
    memset(pushed,0,sizeof(pushed));
    q.push(1);
    pushed[1]=1;
    while(!q.empty()){
        int u=q.front();
        q.pop();
        pushed[u]=0;
        for(int v=head[u];v!=-1;v=edge[v].next){
            if(d[edge[v].v]>edge[v].w+d[u]){
                d[edge[v].v]=edge[v].w+d[u];
                q.push(edge[v].v);
            }
        }
    }
    return;
}


int main(){
    memset(head,-1,sizeof(head));
    k=0;
    int u,v,w;
    scanf("%d%d",&n,&m);
    for(int i=1;i<=m;++i){
        scanf("%d%d%d",&u,&v,&w);
        add(u,v,w);
        add(v,u,w);
    }
    SPFA();
    
    printf("%d",d[n]);
    
    return 0;
}
```

# 最短路径中的一些重要性质

### 三角不等式 

对任意边 $\text{<}u,v\text{>}\in E$，有$\delta(s,v)\leq \delta(s,u)+w(u,v)$

### 上界性质

对于任意顶点 $u\in V$，有$d[v]\geq \delta(s.v)$，而且一旦 $d[v]$ 到达，$\delta(s,v)$ 就不再改变。

### 无路径性质

如果从 $s$ 到 $v$ 不存在路径，则总是有 $d[v]=\delta(s,v)=\infty$

### 收敛性质

如果 $s\leadsto u\rightarrow v$ 是图 $G$ 某 $u,v\in V$ 的最短路径，而且在松弛边 $\text{<}u,v\text{>}$ 之前的任意时间 $d[u]=\delta(s,u)$，则在操作之后总有 $d[v]=\delta(s,v)$

### 路径松弛性质

如果 $p=\text{<}v_0,v_1,\ldots ,v_k\text{>}$ 是从 $s=v_0$ 到 $v_k$ 的最短路径，而且 $p$ 的边按照 $(v_0,v_1)(v_1,v_2),\ldots,(v_{k-1},v_k)$ 的顺序进行松弛，那么 $d[v_k]=\delta(s,v_k)$。这个性质的保持并不受其他松弛操作的影响，即使它们与 $p$ 边上的松弛操作是混合在一起也是一样的。

### 前驱子图性质

一旦对于所有 $v\in V,d[v]=\delta(s,v)$，前驱子图就是一个以 $s$ 为根的最短路径树。