---
title: DFS
author: saltycat
date: 2022-04-14 17:13:37 +0800
categories: [图论,算法]
tags: [DFS,图的遍历,最短路,寻路]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io

---

## DFS (depth-first-search)

DFS 策略为：从一结点出发，挑选相邻结点进行搜索，若没有可走（没有结点或者是结点已搜索）结点，则回退至上一个结点继续搜索。

## 寻路

如何利用 DFS 进行寻路？

```C++
bool dfs(s, d) { // s 为起点，d 为终点 
	if( s 为 d ) // 找到终点了
		return true;
	if( s 为旧点 ) // s 已访问过
		return false;
	将 s 标记为旧点; // 没访问过 s，标记为访问过
    对和 s 相邻的每个节点 U { 
		if( dfs(U, d) == true)
			return true;
	}
	return false; 
}
```

若要记录路径，则需要引入一个路径数组：

```c++
Node path[MAX_LEN]; // MAX_LEN取节点总数即可
int depth;
bool dfs(V, D) {
	if( V 为 D ){ // 找到终点了
		path[depth] = V; 
		return true;
	}
	if( V 为旧点 ) // 已访问
		return false;
	将 V 标记为旧点;
	path[depth]=V; // 假设这一条路可以走，记录下当前结点
	++depth;
	对和 V 相邻的每个节点 U {  // 深入
		if(dfs(U, D) == true) // U 可以到终点，则 V 也一定可以到终点
			return true;
	}
    // 找不到终点，需要回溯到上一结点
	--depth; // 撤销当前记录的结点
	return false;
}
int main()
{
	将所有点都标记为新点;
	depth = 0;
	if( dfs(起点, D)) {
		for(int i = 0;i <= depth; ++ i)
		cout << path[i] << endl;
	}
}

```

## 遍历

```c++
void dfs(V) {
	if( V是旧点 )
		return;
	将 V 标记为旧点;
	对和 V 相邻的每个点 U {
		dfs(U);
	} 
}
int main() {
	将所有点都标记为新点;
	while(在图中能找到新点k) 
		dfs(k);
}

```

## 图的储存

### 邻接矩阵

用一个二维数组 `G[i][j]` 来储存图，其值表示结点 $i,j$ 之间的连接情况（如有边、无边、权重、方向等）

遍历复杂度：$\mathcal{O}(N^2)$

![矩阵储存图](/assets/blog_res/2022-04-14-graph-again.assets/矩阵储存图.png)

### 邻接表

利用链表或 vector 实现。

遍历复杂度：$\mathcal{O}(N+E)$

![邻接表储存图](/assets/blog_res/2022-04-14-graph-again.assets/邻接表储存图.png)
