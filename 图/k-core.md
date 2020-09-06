# k-core

k-core：图G中的极大子图Gk，Gk中的所有顶点的度deg(v) ≥ k
core number：一个顶点属于k-core，但不属于(k+1)-core，则这个顶点的core number=k

k-core定义的是一种子图结构，在这个子图中，每个顶点至少有k个相邻顶点。因此在较大的k的情况下，k-core可以认为是一个顶点之间联系紧密的子图，但这个子图并不一定连通。类似的定义还有k-truss

性质：
1.对于1 ≤ k < kmax，Gk+1是Gk的子图
2.k-core不一定是连通子图
3.Gk是所有core number ≥ k的顶点的诱导子图

core composition：计算图中所有的k-core。根据性质3，这一问题等价于计算所有顶点的core number
算法在论文 An O(m) Algorithm for Cores Decomposition of Networks 中给出，代码如下：

```c++
#include <bits\stdc++.h>
using namespace std;
vector<int> *graph;  //图的邻接链表
int n, dmax;   //n为最大顶点编号,m为边数量,dmax为最大度
set<pair<int, int>> edge;    //边的集合
vector<int> core;    //记录每个点的core number
vector<int> degree;  //记录每个点的度
void InitGraph(char* file)   //建立图
{
	FILE* f = fopen(file, "r");
	int a, b;  
	//计算最大顶点编号为n
	while (fscanf(f, "%d %d", &a, &b) != EOF)
	{
		if (a > n) n = a;
		if (b > n) n = b;
	}
	rewind(f);
	graph = new vector<int>[n+1];
	core.resize(n+1);
	degree.resize(n+1);
	while (fscanf(f, "%d %d", &a, &b) != EOF)
	{
		if (a == b) continue;  //忽略指向自己的边
		if (a < b)  //令a>b
		{
			int t = a;
			a = b;
			b = t;
		}
		if (edge.find(pair<int, int>(a, b)) == edge.end())  //这条边目前不存在，避免重边
		{
			graph[a].push_back(b);
			graph[b].push_back(a);
			edge.insert(pair<int, int>(a, b));
		}
	}
	fclose(f);
	//计算顶点的度
	for (int i = 0; i <= n; ++i)
	{
		int t = graph[i].size();  
		degree[i] = t;
		if (t > dmax)
			dmax = t;
	}
	
}

void core_decomposition()
{
	vector<int> *bin = new vector<int>[dmax + 1];  //用于桶排序的桶
	for (int i = 0; i <= n; ++i)
		bin[degree[i]].push_back(i);
	vector<bool> del(n + 1, false);  //记录顶点是否已经删除
	for (int i = 0; i <= dmax; ++i)   //遍历所有的bin
	{
		for (int j = 0; j < bin[i].size(); ++j)  //这一bin中所有元素
		{
			int t = bin[i][j];  //顶点的编号
			if (del[t] == true) continue;  //已删除则不用考虑
			del[t] = true;
			core[t] = degree[t];
			//与顶点t相邻的所有顶点
			for (int k = 0; k < graph[t].size(); ++k)
			{
				int u = graph[t][k];  //顶点u
				if (del[u] == true) continue;
				if (degree[u] > i)  //u的core还没计算过
				{
					--degree[u];
					bin[degree[u]].push_back(u);  //u放到前面的bin中
				}
			}
		}
	}
	delete []bin;
}

void k_core(int K) //计算指定k值得k-core
{
	queue<int> q;
	vector<bool> del(n+1, false);  //顶点是否已删除
	for (int i = 0; i <= n; ++i)
	{
		if (degree[i] < K)
			q.push(i);
	}
	while (q.empty() == false)
	{
		int u = q.front();
		q.pop();
		del[u] = true;
		for (int i = 0; i < graph[u].size(); ++i)
		{
			int v = graph[u][i];
			if (del[v] == true) continue;
			degree[v]--;
			if (degree[v] < K)
				q.push(v);
		}
	}
	//输出结果，剩下的顶点是k-core
	for (int i = 0; i <= n; ++i)
	{
		if (del[i] == false)
			printf("%d ", i);
	}
}
```

另一种core decomposition算法
基本思想：在顶点u的所有相邻顶点Nbr(u)中，core number大于等于k的个数>=k个，满足这种条件的最大的k值就是点u的core number。

对照谷歌学术中的h指数 (h-index) 的概念，core(u) = h-index( core( Nbr(u) ) )

基于这种方法，可以用迭代的算法计算core decomposition。所有顶点的core number初始化为顶点的度，每一轮循环中按照上面的方法重新计算、更新顶点的core number。当所有顶点的core都不改变时，算法结束。

使用这种方法不需要知到图的全部信息，因此可以更容易地设计出并行算法和I/O算法。

算法改进：
定义变量cnt(u)：顶点u的相邻顶点v中，core(v)>=core(u)的v的个数。
在一次迭代中，只有当core(u)>cnt(u)时，才需要重新计算core(u)。

```c++
int h_index(int u)   //计算u的相邻顶点的h-index
{
	int size = graph[u].size();
	vector<int> cnt(size + 1, 0);
	for (int i = 0; i < graph[u].size(); ++i)
	{
		int v = graph[u][i];
		if (core[v] > size)
			cnt[size]++;
		else
			cnt[core[v]]++;
	}
	for (int i = size; i >= 1; --i)
	{
		if (cnt[i] >= i)
			return i;
		else
			cnt[i - 1] += cnt[i];
	}
}

void core_decomposition_hindex()
{
	queue<int> q;
	queue<int> q2;
	vector<int> cnt(n + 1, 0);
	vector<bool> in_q(n + 1, false);   //记录顶点是否在q中
	for (int i = 0; i <= n; ++i)
	{
		core[i] = degree[i];
		for (int j = 0; j < graph[i].size(); ++j)
		{
			if (core[j] > core[i])
				++cnt[i];
		}
		if (cnt[i] < core[i])
		{
			q.push(i);
			in_q[i] = true;
		}
	}
	while (q.empty() == false)
	{
		while (q2.empty() == false) q2.pop();   //下一轮要计算的点
		while (q.empty() == false)
		{
			int u = q.front();
			q.pop();
			in_q[u] = false;
			int old = core[u];
			core[u] = h_index(u);   //计算新的core(u)
			cnt[u] = 0;
			for (int i = 0; i < graph[u].size(); ++i)
			{
				int v = graph[u][i];
				if (core[v] >= core[u])   //重新计算cnt(u)
					cnt[u]++;
				if (in_q[v] == false && core[u] < core[v] && core[v] <= old)  //之前core(v)<=core(u),计算之后core(u)<core(v)了。
				{
					if (cnt[v] == core[v])
					{
						q2.push(v);
						in_q[v] = true;
					}
					cnt[v]--;
				}
			}
		}
		q = q2;
	}
}
```

