---
title: spfa
date: 2017-09-11 20:31:35
tags: [algorithm,图论,]
---
## 前置技能
- [前向星](https://skphetz.github.io/2017/09/11/%E5%89%8D%E5%90%91%E6%98%9F/)
- 图论相关

## 算法简介
　　全称Shortest Path Faster Algorithm，可以解决存在负权边的最短路问题（但无法解决存在负权环的图），复杂度上比Bellman-ford要优。用一个数组dist记录每个节点最短路径的估计值，用前向星存图，设立一个队列保存待更新的点，更新时依次取出队首节点，用该节点估计值来更新与之联通的点v（松弛操作），如果v的最短路估计值有所更新而v不在队列中，则将v加入队尾。重复执行以上操作直到队列中没有元素。期望复杂度Θ(ke)，k为每个顶点入队的次数（一般<=2），e为顶点个数。
<!--more-->
## 算法流程
1. 初始化dist.
2. 利用前向星存图。
3. 取出队首元素，对该点所连的边进行松弛计算，将更新过的节点加入队列。
4. 重复3，直到队列中没有元素。

## 主要代码
```cpp
void spfa(int s)
{
	int u,v;
	memset(vis,0,sizeof(vis));
	for(int i=1; i<=n; i++)
		d[i]=inf;
	f=r=d[s]=0;
	q[++r]=s;
	vis[s]=1;
	while(f!=r)
	{
		f=(f+1)%n;
		u=q[f];
		for(int c=head[u]; c; c=nxt[c])
		{
			v=to[c];
			if(d[u]+w[c]<d[v])
			{
				d[v]=d[u]+w[c];
				if(!vis[v])
				{
					r=(r+1)%n;
					q[r]=v;
					vis[v]=1;
				}
			}
		}
		vis[u]=0;
	}
}
```
