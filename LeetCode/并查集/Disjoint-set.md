# Disjoint-Set

并查集(Disjoint-Set)是一种可以动态维护若干个不重叠的集合，并支持*合并*与*查询*两种操作的一种数据结构。

**1. 合并(Union/Merge)[1]：**合并两个集合。 

**2. 查询(Find/Get)：**查询元素所属集合。 

实际操作时，我们会使用一个点来代表整个集合，即一个元素的根结点(可以理解为父亲)。

我们建立一个数组`fa[ ]`表示一个并查集，`fa[i]`表示`i`的父节点。 

**初始化：**每一个点都是一个集合，因此自己的父节点就是自己`fa[i]=i` 

**查询：**每一个节点不断寻找自己的父节点，若此时自己的父节点就是自己，那么该点为集合的根结点，返回该点。 

**修改：**合并两个集合只需要合并两个集合的根结点，即`fa[RootA]=RootB`，其中`RootA`,`RootB`是两个元素的根结点。







## 题目描述

某市调查城镇交通状况，得到现有城镇道路统计表。表中列出了每条道路直接连通的城镇。市政府 "村村通工程" 的目标是使全市任何两个城镇间都可以实现交通（但不一定有直接的道路相连，只要相互之间可达即可）。请你计算出最少还需要建设多少条道路？

## 输入格式

输入包含若干组测试测试数据，每组测试数据的第一行给出两个用空格隔开的正整数，分别是城镇数目 nn 和道路数目 mm ；随后的 mm 行对应 mm 条道路，每行给出一对用空格隔开的正整数，分别是该条道路直接相连的两个城镇的编号。简单起见，城镇从 11 到 nn 编号。

注意：两个城市间可以有多条道路相通。

## 输出格式

对于每组数据，对应一行一个整数。表示最少还需要建设的道路数目。



```c++
#include <bits/stdc++.h>
using namespace std;
int pre[1000001],n,m,ans;
inline int Find(int x){
	return pre[x]==x?x:pre[x]=Find(pre[x]);
}
inline void Union(int x, int y){
	int fx=Find(x),fy=Find(y);
	if(fx!=fy) pre[fx]=fy;
}
int main()
{
    while(scanf("%d",&n)&&n){
        ans=0;
        scanf("%d", &m);
        for(int i=1;i<=n;i++) pre[i]=i;
        for(int i=1,x,y;i<=m;i++){
            scanf("%d%d",&x,&y);
            Union(x,y); 
        }
        for(int i=1;i<=n;i++){
            if(Find(i)==i) ans++;
        }
        printf("%d\n",ans-1);
    }
    return 0;
}
```



## 题目描述

妈妈下班回家，街坊邻居说小明被一群陌生人强行押上了警车！妈妈丰富的经验告诉她小明被带到了 tt 区，而自己在 ss 区。

该市有 mm 条大道连接 nn 个区，一条大道将两个区相连接，每个大道有一个拥挤度。小明的妈妈虽然很着急，但是不愿意拥挤的人潮冲乱了她优雅的步伐。所以请你帮她规划一条从 ss 至 tt 的路线，使得经过道路的拥挤度最大值最小。

## 输入格式

第一行有四个用空格隔开的 nn，mm，ss，tt，其含义见【题目描述】。

接下来 mm 行，每行三个整数 u, v, wu,v,w，表示有一条大道连接区 uu 和区 vv，且拥挤度为 ww。

**两个区之间可能存在多条大道**。

## 输出格式

输出一行一个整数，代表最大的拥挤度。

看到**求最大值的最小**我们知道一定会用二分解决。 顺着这个思路走，我们可以二分这个拥挤度，在判断这个拥挤度是否可行时，把所有拥挤度大于mid的边都去掉，最后并查集判断`s`点与`t`点是否联通即可。

```c++
#include<bits/stdc++.h>
#define INF 0x3f3f3f3f
#define N 50050
using namespace std;
int l=INF,r=-1,n,m,s,t,ans;
int pre[N],x[N],y[N],cost[N];
inline int find(int x){return x==pre[x]?x:pre[x]=find(pre[x]);}
inline int check(int mid){
	for(int i=1;i<=n;i++) pre[i]=i;
	for(int i=1;i<=m;i++){
		if(cost[i]>mid) continue;
		int fx=find(x[i]),fy=find(y[i]);
		if(fx!=fy) pre[fx]=fy;
	}
	if(find(s)==find(t)) return 1;
	return 0;
}
int main()
{
	scanf("%d%d%d%d",&n,&m,&s,&t);
	for(int i=1;i<=m;i++){
		scanf("%d%d%d",&x[i],&y[i],&cost[i]);
		l=min(l,cost[i]);r=max(r,cost[i]);
	}
	while(l<=r){
		int mid=(l+r)>>1;
		if(check(mid)){
			r=mid-1;
			ans=mid;
		}
		else l=mid+1;
	}
	printf("%d",ans);
	return 0;
}
```



## 题目描述

Caima 给你了所有 $[a,b]$ 范围内的整数。一开始每个整数都属于各自的集合。每次你需要选择两个属于不同集合的整数，如果这两个整数拥有大于等于 p 的公共质因数，那么把它们所在的集合合并。

重复如上操作，直到没有可以合并的集合为止。

现在 Caima 想知道，最后有多少个集合。

## 输入格式

一行，共三个整数 a,b,p用空格隔开。

## 输出格式

一个数，表示最终集合的个数。

**输入**

```
10 20 3
```

**输出**

```
7
```

## 说明/提示

#### 样例 1 解释

对于样例给定的数据，最后有 \{10,20,12,15,18\},\{13\},\{14\},\{16\},\{17\},\{19\},\{11\}共 7 个集合，所以输出应该为 7。

```c++
#include<iostream>
 
using namespace std;
 
const int N = 100000 + 10;
 
int prime[N],pre[N];
 
int find(int x)
{
	if(x != pre[x])
		pre[x] = find(pre[x]);
	return pre[x];
}
 
int main()
{
	int a,b,p;
	cin>>a>>b>>p;
	int ans = b - a + 1;//合并一个就减一 
	for(int i=a; i<=b; i++) pre[i] = i;
	for(int i=2; i<=b; i++)
	{
		if(!prime[i])
		{
			if(i >= p)
			{
				for(int j=i+i; j<=b; j+=i)
				{
					prime[j] = 1;
					if(j - i >= a && find(j) != find(j-i))//是同一个素数的倍数就合并 
					{
						pre[find(j)] = find(j-i);
						ans--;
					}
				}
			}
			else
			{
				for(int j=i*2; j<=b; j+=i)
					prime[j] = 1;
			}
		}		
	}
	cout<<ans;
	return 0;
}
 
```

