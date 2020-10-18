# STL 常见数据类型与函数

## Vector

#### 初始化

```c++
vector<int> v; // 空数组
vector<int> v{0,1,2,3,4,5};
vector<int> v(n, 0); //长度为n的数组
vector<vector<int> > v(m, vector<int>(n, 0));//m*n的矩阵初始化
vector<int> u(v); 复制构造函数
vector<int> u(v.begin()+3,v.end()):复制[begin,end)区间内另一个数组的元素到vector中
```

#### 插入删除

```c++
v.push_back(0);
v.insert(iterator it,const T& x);//向量中迭代器it指向元素前增加一个元素x
v.insert(iterator it,int n,const T& x);//向量中迭代器指向元素前增加n个相同的元素x
v.insert(iterator it,const_iterator first,const_iterator last);//向量中迭代器指向元素前插入另一个相同类型向量的[first,last)间的数据

v.erase(iterator it); //删除向量中迭代器指向元素
v.erase(iterator first,iterator last); //删除向量中[first,last)中元素
v.pop_back(); //删除向量中最后一个元素
v.clear(); //清空向量中所有元素
```



#### 其他常规函数

```c++
v.front(); //==v[0]
v.back();  //==v[v.size()-1]
v.empty(); //判断是否为空
v.size(); //容器中有多少元素
v.resize(); //强制把容器改为包含n个元素。调用resize之后，size将会返回n。如果n小于当前大小，容器尾部的元素会被销毁。如果n大于当前大小，新默认构造的元素会添加到容器尾部。
v.capacity(); //容器在它已经分配的内存中可以容纳多少元素
v.reserve();//强制容器把它的容量改为至少n

v.assign(int n,const T& x); //赋值n个元素的值为x

swap(v[i], v[j]); //交换两个元素的位置
sort(v.begin(),v.end());//从小到大排序
sort(v.begin(),v.end(), greater<int>());
reverse(v.begin() + i, v.end()); //镜像翻转

//正序的数组
int idx = upper_bound(v.begin(), v.end(), x) - v.begin(); //返回第一个大于x的元素下标，等价于二分查找
int idx = lower_bound(v.begin(), v.end(), x) - v.begin(); //返回第一个大于或等于x的元素下标

//逆序的数组
int pos3=lower_bound(v.begin(), v.end(),x,greater<int>())-v.begin();  //返回数组中第一个小于或等于被查数的值 
int pos4=upper_bound(v.begin(), v.end(),x,greater<int>())-v.begin();  //返回数组中第一个小于被查数的值
```



## list

```c++
list<int> l;
list<int> l(n, 0); //长度为n的链表
list<int> l(v.begin(), v.end());
pop_back(); // 删除最后一个元素 
pop_front(); // 删除第一个元素 
push_back(); // 在list的末尾添加一个元素 
push_front(); // 在list的头部添加一个元素

//splice实现list拼接的功能。将源list的内容部分或全部元素删除，拼插入到目的list。
splice ( iterator position, list<T,Allocator>& x );
splice ( iterator position, list<T,Allocator>& x, iterator it );
splice ( iterator position, list<T,Allocator>& x, iterator first, iterator last );
//对于一：会在position后把list&x所有的元素到剪接到要操作的list对象
//对于二：只会把it的值剪接到要操作的list对象中
//对于三：把first 到 last 剪接到要操作的list对象中

l.sort(); //给list排序 
l.merge(x); //合并x链表到l

//其他函数和vector一样

```



## unordered_set、set、multiset

unordered_set基于哈希表，是无序的。存储无重复值的集合。
set实现了红黑树的平衡二叉检索树的数据结构，插入元素时，它会自动调整二叉树的排列，把元素放到适当的位置，以保证每个子树根节点键值大于左子树所有节点的键值，小于右子树所有节点的键值；另外，还得保证根节点左子树的高度与右子树高度相等。

平衡二叉检索树使用中序遍历算法，检索效率高于vector、deque和list等容器，另外使用中序遍历可将键值按照从小到大遍历出来。

multiset与set不同的是可以存储重复值。

#### 初始化

```c++
unordered_set<int> st(nums.begin(), nums.end());//使用vector初始化
set<int> st(nums.begin(), nums.end());
set<int> numbers {8, 7, 6, 5, 4, 3, 2, 1};默认为升序 operator <. less<int>
multiset<int, greater<int> > st(nums.begin(), nums.end());  降序
```



#### 常规函数（两者相同）

```c++
st.begin();
st.end();
st.insert(3);//插入元素3
st.emplace(3);//插入
st.erase(3);//删除元素3
unordered_set<int>::iterator iter = st.find(3);//查找元素3，返回迭代器。
st.find(3) != st.end(); //判断是否存在3.
st.count(3); //判断是否存在3，返回0或1

// 利用set去重
vector<int> vec;
vec = { 1, 2, 3, 4, 8, 9, 3, 2, 1, 0, 4, 8 };
set<int> st(vec.begin(), vec.end());
vec.assign(st.begin(), st.end());
```

### 

## unordered_map和map

+ map内部自建一颗红黑树（一种非严格意义上的平衡二叉树），所以在map内部所有的数据都是有序的，且map的查询、插入、删除操作的时间复杂度都是O(logN)



+ unordered_map和map类似，都是存储的key-value的值，可以通过key快速索引到value。不同的是unordered_map不会根据key的大小进行排序，存储时是根据key的hash值判断元素是否相同，即unordered_map内部元素是无序的。哈希表最大的优点，就是把数据的存储和查找消耗的时间大大降低，时间复杂度为`O(1)`；而代价仅仅是消耗比较多的内存。哈希表的查询时间虽然是O(1)，但是并不是unordered_map查询时间一定比map短，因为实际情况中还要考虑到数据量，而且unordered_map的hash函数的构造速度也没那么快，所以不能一概而论，应该具体情况具体分析。

```c++
//unordered_map和map在下面各函数定义一致
map<string,int> m; //声明
unordered_map<string,int> m;

//添加元素
m[string("one")] = 1;
m.insert(pair<string,int>( "two",2));

//遍历,根据key排序的，key是string，故是字典序排序，从a-z
map< string,int>::iterator iter;
for(iter = m.begin(); iter != m.end(); iter++)
    cout<<iter->first<<' '<<iter->second<<endl;//输出的是key value值

//反向迭代，从z-a
map<string,int>::reverse_iterator iter;
for(iter = m.rbegin(); iter != m.rend(); iter++)
    cout<<iter->first<<' '<<iter->second<<endl;

//查找
iter = m.find(string("one"));
if(iter!=m.end()) cout<<iter->second<<endl;
if(m.count(string("one")) == 0); //没找到

//删除
m.erase(iter);//删除一个条目
m.erase(string("one"));//根据键值删除


//使unordered_map支持pair作为键值
struct pair_hash
{
    template<class T1, class T2>
    std::size_t operator() (const std::pair<T1, T2>& p) const
    {
        auto h1 = std::hash<T1>{}(p.first);
        auto h2 = std::hash<T2>{}(p.second);
        return h1 ^ h2;
    }
};
unordered_map<pair<int, bool>, int, pair_hash> Map;


```

## queue和priority_queue

```c++
//定义
queue<int> q;

q.push(x); //入队
q.pop(); //出队
q.front(); //访问队首
q.back(); //访问队尾
q.size(); //队中元素个数
q.empty();

deque<int> q;
q.push_front();
q.push_back();
q.pop_front();
q.pop_back();
```

priority_queue(优先队列），优先队列与队列的差别在于优先队列不是按照入队的顺序出队，而是按照队列中元素的优先权顺序出队（默认为大者优先，也可以通过指定算子来指定自己的优先顺序）。

priority_queue模版类有三个模版参数，元素类型，容器类型，比较算子。其中后两个都可以省略，默认容器为vector，默认算子为less，大顶堆。

```c++
//下面两个都是降序队列 排序与sort函数相反
priority_queue<int> q;
priority_queue<int,vector<int>,less<int>> q;

//升序队列
priority_queue <int,vector<int>,greater<int> > q;

// 使用lambda表达式比较元素
    auto cmp = [](int left, int right) {return (left ^ 1) < (right ^ 1);};
    std::priority_queue<int, std::vector<int>, decltype(cmp)> q3(cmp);

q.top(); //访问队首
//其他函数与queue类似
```

## stack

```c++
stack<int> s;

s.push(x); //入栈
s.pop(); //出栈
s.top(); //访问栈顶
s.size(); //栈中元素个数
s.empty();
```

## bitset

```c++
#include <bitset>
bitset<64> k; //64位二进制形式数,默认每一位为0
bitset<64> k(12); //转为二进制数，前面补0
bitset<64> k("010111"); 
k.set(); //所有位 置为1
k.set(i); //第i位 置为1
k.set(i,0); //第i位 置为0
k.reset(); //所有位 置为0
k.reset(i); //第i位 置为0
k.flip(); //所有位取反
k.flip(i); //第i位取反
long long v = k.to_ullong();
long v = k.to_ulong();
string v = k.to_string();
k.count(); //返回1的个数
l.test(i); //第i位为0或1，对应返回false或true
l.any(); // any函数检查bitset中是否有１,对应返回false或true
l.all(); // all函数检查bitset中是全部为１
l.none(); //none函数检查bitset中是否没有１
```



