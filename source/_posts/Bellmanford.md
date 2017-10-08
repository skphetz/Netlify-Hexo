---
title: Bellmanford
date: 2017-09-11 20:31:52
tags: [algorithm,图论]
---
## 前置技能
- [前向星](https://skphetz.github.io/2017/09/11/%E5%89%8D%E5%90%91%E6%98%9F/)
- 图论相关

## 算法简介
　　求最短路的算法之一，类似于Dijkstra算法，对于每一个点集中的点v，逐步减少起点s到v的最短路长度的估计值，知道达到其最短路径值。同时，Bellman-ford算法还会返回一个bool类型的值如果不存在从s可达到的负权回路就返回true，否则false，这样就能够在存在负权边的情况下解决单源最短路的问题，这点上比Dijkstra算法要优。由于该算法更多的是对边进行操作，因此能更高效地解决稀疏图的最短路问题。算法复杂度Θ(V*E)。
<!--more-->
## 算法流程
1. 初始化dist为无穷大（只有原点初始化为0）。
2. 利用前向星存图。
3. 进行循环，遍历所有边，进行松弛计算。
4. 遍历中途的所有边，判断是否存在dist[v]>dist[u]+w[u][v]，如果有则返回false，表示从原点出发存在可达的负权回路。

## 主要代码
```cpp
bool Bellman_ford(int s)
{
	int tt;
	for(int i=1; i<=n; i++)
		dist[i]=Inf;
	dist[s]=0;
	for(int i=1; i<=n-1; i++)
		for(int j=1; j<=n; j++)
		{
			if(dist[j]==Inf)
				continue;
			for(int k=head[j]; k+1; k=nxt[k])
			{
				tt=to[k];
				if(w[k]!=Inf&&dist[tt]>dist[j]+w[k])
					dist[tt]=dist[j]+w[k];
			}
		}
	for(int j=1; j<=n; j++)
	{
		if(dist[j]==Inf)
			continue;
		for(int k=head[j]; k+1; k=nxt[k])
		{
			tt=to[k];
			if(w[k]!=Inf&&dist[tt]>dist[j]+w[k])
				return 0;
		}
	}
	return 1;
}
```
