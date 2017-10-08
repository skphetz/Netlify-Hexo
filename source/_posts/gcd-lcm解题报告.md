---
title: GCD&LCM解题报告
date: 2017-09-11 20:21:27
tags: [algorithm,数论]
---
# 题面

## 【问题描述】
给定一个由N个整数构成的数列{A}，
求出如下描述的两个最长子串：
使得GCD(Ai,Ai+1,…,Aj)=1，其中 (1 ⩽ i ＜ j ⩽ N)，其串长作为参数 α。
满足LCM(Ai,Ai+1,…,Aj)=AiAi+1…Aj，其中 (1 ⩽ i ＜ j ⩽ N)，其串长作为参数 β。
<!--more-->
## 【输入格式】
对于每个测试点：
第一行包括一个整数 T，代表数据组数。
对于接下来的每一组数据，包括两行。
第一行，为一个整数 N 代表序列长度。
第二行，为用空格分隔的 N 个整数 Ai。

## 【输出格式】
对于第 i 组数据，你需要输出组数标示 “Case i: ” 其中 i 表示当前的数据组数。
紧接着，需要输出所要计算的参数α与β，以空格分隔。
如果不存在所要求的子串，对应的参数α或β设为 -1。

## 【输入样例】
```
3
2
7 2
4
2 2 3 4
3
2 2 4
```

## 【输出样例】
```
Case 1: 2 2
Case 2: 4 2
Case 3: -1 -1
```

## 【数据范围】
对于 20% 的数据, T = 1, N ⩽ 10, Ai ⩽ 10(1 ⩽ i ⩽ N)。
对于 50% 的数据, T ⩽ 10, N ⩽ 100, Ai ⩽ 100(1 ⩽ i ⩽ N)。
对于 70% 的数据, T ⩽ 20, N ⩽ 1000, Ai ⩽ 1000(1 ⩽ i ⩽ N)。
对于 100% 的数据, 1 ⩽T ⩽ 30, 1 ⩽ N ⩽ 10^5 , 1 ⩽ Ai ⩽ 10^6 (1 ⩽ i ⩽ N)。

## 【题目来源】
雅礼2014模拟赛18

# 题解
## 关于第一问
　　第一问肥肠简单，根据观察以及一些思考可以得到，如果数列A中存在gcd(a[i],a[i+1])==1则α=n，否则无解α=-1。
　　![](1.jpg)
## 重点在第二问
　　首先想到动态规划。令f[i]表示以a[i]结尾，满足条件的最长序列长度。容易想到转移方程f[i]=min(f[i-1]+1,i-k+1)，其中k表示最后一个不与a[i]互质的数的序号。但是这时候有一个问题，那就是如何处理出k……(*￣︿￣)。因为数据范围比较大，使用n^2的枚举肯定超时。
　　![](2.jpg)
　　好的分析完了#滑稽）。我们先预处理出10^6每个数的最大的质因子fac[i]，当指针下标指在i时，令num=a[i]，我们就可以通过不断地除来将num分解质因数，每分解出一个质因数，我们用vis[]来保存这个质因数最后出现的位置，这样就可以通过取max来确定最后一个不与a[i]互质的数的序号，用las保存。代码如下：
```cpp
for(int i=1; i<=n; i++)
{
	int num=a[i],las=0;
	while(num>1)
	{
		int p=fac[num];             //取出num的最大质因数
		while(fac[num]==p)
			num=num/p;
		las=max(las,vis[p]);        //更新最后一个不与a[i]互质的数的序号
		vis[p]=i+1;                 //更新p这个因数最后一次出现的位置
	}
	if(i==1)
		f[i]=1;
	else
		f[i]=min(f[i-1]+1,i+1-las); //转移
}
```
　　而我们最后的答案就是max{f[1],f[2],…,f[n]}。可以用一个函数
```cpp
ans2=*max_element(f+1,f+n+1);
```
　　这个函数的作用就是取出f数组1~n下标中的最大值（长知识），同理的还有一个*min_element。
　　最后的事情就是输出了。注意空格233。
# 完整代码
```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;
const int N=1000020;
int T,n,ans1,ans2,tot;
int a[N],prime[N],f[N],fac[N],vis[N];
int read()
{
	int x=0,f=1;char ch=getchar();
	while(!isdigit(ch)) {if(ch=='-') f=-1; ch=getchar();}
	while(isdigit(ch)) {x=x*10+ch-'0'; ch=getchar();}
	return x*f;
}
void write(int x)
{
	if(x<0) putchar('-'),x=-x;
	if(x>=10) write(x/10);
	putchar(x%10+'0');
}
void writeln(int x)
{
	write(x);puts("");
}
inline int gcd(int a,int b){return b==0?a:gcd(b,a%b);}
int main()
{
	for(int i=2; i<N; i++)
	{
		if(!fac[i])
			prime[tot++]=i,fac[i]=i;
		for(int j=0; j<tot; j++)
		{
			if(i*prime[j]>N)
				break;
			fac[i*prime[j]]=fac[i];
			if(i%prime[j]==0)
		  		break;
	  	}
	}
	T=read();
	for(int t=1; t<=T; t++)
	{
		printf("Case %d: ",t);
		n=read();
		for(int i=1; i<=n; i++)
			a[i]=read();
		ans1=ans2=-1;
		int k=gcd(a[1],a[2]);
		for(int i=3; i<=n; i++)
			k=gcd(k,a[i]);
		if(k==1)
			ans1=n;
		memset(vis,0,sizeof vis);
		for(int i=1;i<=n; i++)
		{
			int num=a[i],las=0;
			while(num>1)
			{
				int p=fac[num];
				while(fac[num]==p)
					num=num/p;
				las=max(las,vis[p]);
				vis[p]=i+1;
			}
			if(i==1)
				f[i]=1;
			else
				f[i]=min(f[i-1]+1,i+1-las);
		}
		ans2=*max_element(f+1, f+n+1);
		if(ans2==1)
			ans2=-1;
		write(ans1),putchar(' '),writeln(ans2);
	}
	return 0;
}
```
~~完结撒花~~
![](3.gif)
