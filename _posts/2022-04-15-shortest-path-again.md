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

感谢邓俊辉老师！

声明一下 Dijkstra 的音标：/ˈdɛɪkstra/

假设你有一串珠子，珠子之间通过长度不同的线段连接而成，如何找出任意两珠子之间的最短路径？

答案是，用手抓住两颗珠子，逐渐伸直，直线段的长度即为最小路径长度，线段上的珠子即为经过的节点。（如图所示）

![模拟求最短路径](/assets/blog_res/2022-04-15-shortest-path-again.assets/image-20220417191031010.png)

如何模拟这一个过程呢？

如下图所示，假设有这么一个图，平摊在桌面上，先要找出从源点 S 到其他各个点的最短距离。仅需提起 S 点，其余的点逐个绷直地被提起来，即可求得最短路径。

![image-20220417194920746](/assets/blog_res/2022-04-15-shortest-path-again.assets/image-20220417194920746.png)

我们不妨先提起来一部分点，如图可以看见，整个所有点被划分为了两个部分，被提起来的绿色的部分（已经确定最短路径的），和未被提起来的蓝色部分。注意到边也被分为了三类，未提起来的点与提起来的点相连的粉红色边、提起来的点之间相连的绿色边、为提起来的点之间相连的蓝色边。

其中要注意的，竖直边是最短路径。

如何找出下一个被提起来的点是什么呢？

显然绿色边的状态已经固定了，蓝色边要被绷直必然要先绷直粉红色边，也就是说，下一个被提起来的点必然与粉红色的边相连。因此，我们只需要搜索所有粉红色边（连接两个不同集合的点的边，又称作 bridge），对于任意与粉红色边相连的蓝色结点 $v$，他的最短距离
$$
dist[v]=\min \left\{\begin{matrix}
 dist[v]\\
dist[u]+e<u,v>\\
\end{matrix}\right.
$$
对于所有的蓝色结点中 $dist[v]$ 最小的就是会被提上去的点。

![image-20220417194903158](/assets/blog_res/2022-04-15-shortest-path-again.assets/image-20220417194903158.png)



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



Dijkstra 的流程：

1. 初始化边：
   - `e[i][j] = INF`
   - `e[i][i] = 0`
2. 读入边
3. 对源点 s 进行 Dijkstra
4. 初始化：
   - `dist[i] = e[s][i]` （s 为源点）
   - `v[i] = 0; v[s] = 1` 未被提起来的点（集合 S 的点）
   - `path[i] = -1` ，`path[i]` 用于储存结点 i 的最短距离的前驱点（父节点）
5. 找出未提起来的点当中 dist 最小的那个点 p（即桥最小相连的未被提起来的点）
6. 把 p 收录进集合 S ，即 `v[p] = 1`
7. 更新与他相连且未被提起来的点 v 的 dist，并且记录该节点的前驱点为 p，即 `path[v] = p` 

### 直接扫描未收录顶点

```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<stack>
#define MAXN 1005
#define INF 0x3f // 数量级 1e9，且不用担心 INF + INF 超出 int 范围
using namespace std;
int e[MAXN][MAXN]; 		// 邻接矩阵储存边 
int dist[MAXN];			// 用于记录节点 v 通过集合 S 中点的最小距离 
int v[MAXN];			// 记录是否访问过，访问过即加入了集合 S 中 
int path[MAXN];			// path[v] = u 记录 v 的最小距离前驱点为 u

int n,m,edge; // n 节点数  m 边数  edge 边长 

void Dijkstra(int s){
	// initialize
	for(int i=1;i<=n;i++){
		dist[i]=e[s][i];
		v[i]=0;
		path[i]=-1;
	}
	
	// 把源点 s 加入集合 S
	v[s]=1;
	for(int k=1;k<=n;k++){ // 重复 n 次，保证每个节点都被加入集合 S 
		int min=INF,p;
		// 找出桥中最短的所连接的未访问的点 p
		for(int i=1;i<=n;i++){
		
			for(int j=1;j<=n;j++){
				if(!v[j] && dist[j]<min){
					min=dist[j];
					p=j;
				}
			}
		}
	
		// 把 p 加入集合 S
		v[p]=1;
	
		// 更新与 p 相邻的为加入 S 的点的 dist 和 path
		for(int i=1;i<=n;i++){
			if(!v[i] && dist[i]>dist[p]+e[p][i]){
				dist[i]=dist[p]+e[p][i];
				path[i]=p;
			}
		} 
	}
	
}

int main(){
	scanf("%d%d",&n,&m);
	
	// initialize
	for(int i=1;i<=n;i++){
		for(int j=1;j<=n;j++){
			e[i][j]=(i==j) ? 0 : INF; // 自己到自己距离为 0 
		}
	} 
	
	// 读入边 
	for(int i=1;i<=m;i++){
		int u,v;
		scanf("%d%d%d",&u,&v,&edge);
		e[u][v]=edge;  // 有向图 
	}
	
	// 读入源点 s
	int s;
	scanf("%d",&s); 
	
	Dijkstra(s); 
	
	for(int i=1;i<=n;i++){
		printf("dist[%d]=%d\n",i,dist[i]);
	}
	
	int pnt;
	stack<int> p;

	
	for(int i=1;i<=n;i++){ // 倒叙输出到每个节点的最短路径 
		if(i==s) continue;
		pnt=i;
		printf("%d",i);
		while(path[pnt]!=-1){
			printf("->%d",path[pnt]);
			pnt=path[pnt];
			
		}
		printf("%d\n",s);
	}
	// 需要正序输出可以用栈 
	return 0;
} 
```



