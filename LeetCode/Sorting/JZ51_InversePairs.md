# 数组中的逆序对

### Problem

在数组中的两个数字如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

### Solution

采用归并排序统计逆序对

```c++
int InversePairs(int* data, int length)
{
    if(data == nullptr || length < 0)
        return 0;

    int* copy = new int[length];
    for(int i = 0; i < length; ++i)
        copy[i] = data[i];

    int count = InversePairsCore(data, copy, 0, length - 1);
    delete[] copy;

    return count;
}
int InversePairsCore(int* data, int* copy, int start, int end)
{
    if(start==end)
    {
        copy[start]=data[start];
        return 0;
    }
    int mid = start + (start+end)/2;
    int left = InversePairsCore(copy, data, start, mid);//注意copy和data的位置是相反的，相当于每轮递归将data中的数据暂存入临时数组
    int right = InversePairsCore(copy, data, mid+1, end);
    
    // i初始化为前半段最后一个数字的下标
    int i = start + length;
    // j初始化为后半段最后一个数字的下标
    int j = end;
    int indexCopy = end;
    int count = 0;
	while(i>=start && j>=mid+1)
    {
        if(data[i]>data[j])
        {
            copy[indexCopy--]=data[i--];
            count+=j-mid;
        }
        else
            copy[indexCopy--]=data[j--];
    }
    for(;i>=start;i--)
        copy[indexCopy--]=data[i];
    for(;j>=mid+1;j--)
        copy[indexCopy--]=data[j];
    return left+right+count;
}

```



### Solution2

采用二叉搜索树

```c++
/*二叉树数据结构，新增count属性，保存左节点的个数*/
typedef struct tree
{
	int data;
	int count;//保存当前节点有多少个左节点
	struct tree *left;
	struct tree *right;
	tree(int x):data(x),left(NULL),right(NULL),count(0){}
}Tree,*TreeNode;

/*通过二叉树进行计算*/
void insert(Tree *root, Tree *node, int &small_count) {
	if (node -> data <= root ->data) {
		root -> count ++; //当前节点左节点个数累加，因为插入的节点已经比当前节点小了
		if (root -> left) {
			insert(root->left,node, small_count);
		} else {
			root -> left = node;
		}
	} else {
		/*如果大于当前节点，则说明当前节点的所有左节点包括自己都比插入节点小，进行赋值*/
		small_count += root -> count + 1; 
		if (root ->right) {
			insert(root ->right, node, small_count);
		} else {
			root -> right = node;
		}
	}
}


/*获取最终的逆序数组*/
vector<int> get_smaller_numbers(std::vector<int> &arr) {
	vector<int> count; //逆序数量的数组
	vector<Tree *> node; //二叉树节点数组
	Tree *tmp;

	for (int i  = arr.size() - 1;i >= 0; i--) {
		node.push_back(new tree(arr[i]));
	}

	count.push_back(0);
	for (int i = 1;i < arr.size(); ++i) {
		int small_count = 0;
		insert(node[0],node[i],small_count);
		count.push_back(small_count);
	}
	/*对计算好的结果进行逆序，恢复初始结果*/
	reverse(count.begin(), count.end());
	return count;
}

```







作为程序员的小Q，他的数列和其他人的不太一样，他有



个数。

老板问了小Q一共 m次，每次给出一个整数![img](https://www.nowcoder.com/equation?tex=q_i%20(1%20%3C%3D%20i%20%3C%3D%20m)), 要求小Q把这些数每![img](https://www.nowcoder.com/equation?tex=2%5E%7Bq_i%7D)分为一组，然后把每组进行翻转，小Q想知道每次操作后整个序列中的逆序对个数是多少呢？



例如:

对于序列1 3 4 2，逆序对有(4, 2),(3, 2),总数量为2。

翻转之后为2 4 3 1，逆序对有(2, 1),(4, 3), (4, 1), (3, 1),总数量为4。

```python
def merge(data, lbegin, lend, rbegin, rend, index, order):
    l = lbegin
    r = rbegin
    tmp = []
    pairCnt = 0
    while l <= lend and r <= rend:
        if data[l] <= data[r]:
            tmp.append(data[l])
            l += 1
        else:
            pairCnt += (rbegin-l)
            tmp.append(data[r])
            r += 1
    while l <= lend:
        tmp.append(data[l])
        l+=1
    while r <= rend:
        tmp.append(data[r])
        r+=1
    for i in range(len(tmp)):
        data[lbegin+i] = tmp[i]
    order[index] += pairCnt
 
def mergeSort(data, begin, end, index, order):
    if begin == end:
        return
    mid = (begin+end)//2
    mergeSort(data, begin, mid, index-1, order)
    mergeSort(data, mid+1, end, index-1, order)
    merge(data, begin, mid, mid+1, end, index, order)
 
n = int(input())
data = list(map(int, input().split()))
m = int(input())
q_list = list(map(int, input().split()))
rdata = data[::-1]
order = [0]*(n+1)
rorder = [0]*(n+1)
mergeSort(data, 0, len(data)-1, n, rorder)
mergeSort(rdata, 0, len(rdata)-1, n, order)
 
for q in q_list:
    for i in range(1,q+1):
        order[i], rorder[i] = rorder[i], order[i]
    cnt = 0
    for i in range(1,n+1):
        cnt += rorder[i]
    print(cnt)

```



