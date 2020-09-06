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
    int i = mid;
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





