# 快速排序

### Solution

```c++
void QuickSort (SqList &L)
// 对顺序表 L 进行快速排序
{	QSort ( L, 1, L.length ); 
}  // QuickSort

void QSort(SqList &L, int low, int high)
{
    if(low < high)
    {
        int pivotloc = Partition(L, low, high);
        QSort(L, low, pivotloc-1);
        QSort(L, pivotloc+1, high);
    }
}

int  Partition(SqList &L, int low, int high)
{
    int i=low, j=high;
    int pivotValue = L.data[low];
    while(i < j)
    {
        while(i<j && L.data[j]>=pivotValue) j--;
        L.data[i] = L.data[j];
        while(i<j && L.data[i]<=pivotValue) i++;
        L.data[j] = L.data[i];
    }
    L.data[i] = pivotValue;
    return i;
}
```



## Heap Sort

```c++
#include <iostream>
#include <vector>#include <algorithm>#include <time.h>
#include <Windows.h>
using namespace std;//堆排序的核心是建堆,传入参数为数组，根节点位置，数组长度
void Heap_build(int a[],int root,int length)
{
    int lchild = root*2+1;//根节点的左子结点下标
    if (lchild < length)//左子结点下标不能超出数组的长度
    {
        int flag = lchild;//flag保存左右节点中最大值的下标
        int rchild = lchild+1;//根节点的右子结点下标
        if (rchild < length)//右子结点下标不能超出数组的长度(如果有的话)
        {
            if (a[rchild] > a[flag])//找出左右子结点中的最大值
            {
                flag = rchild;
            }
        }
        if (a[root] < a[flag])
        {
            //交换父结点和比父结点大的最大子节点
            swap(a[root],a[flag]);
            //从此次最大子节点的那个位置开始递归建堆
            Heap_build(a,flag,length);
        }
    }
}
 
void Heap_sort(int a[],int len)
{
    for (int i = len/2; i >= 0; --i)//从最后一个非叶子节点的父结点开始建堆
    {
        Heap_build(a,i,len);
    }
 
    for (int j = len-1; j > 0; --j)//j表示数组此时的长度，因为len长度已经建过了，从len-1开始
    {
        swap(a[0],a[j]);//交换首尾元素,将最大值交换到数组的最后位置保存
        Heap_build(a,0,j);//去除最后位置的元素重新建堆，此处j表示数组的长度，最后一个位置下标变为len-2
    }
 
}
int main(int argc, char **argv)
{
    clock_t Start_time = clock();
    int a[10] = {12,45,748,12,56,3,89,4,48,2};
    Heap_sort(a,10);
     for (size_t i = 0; i != 10; ++i)
     {
         cout<<a[i]<<" ";
     }
    clock_t End_time = clock();
    cout<<endl;
    cout<<"Total running time is: "<<static_cast<double>(End_time-Start_time)/CLOCKS_PER_SEC*1000<<" ms"<<endl;
    cin.get();
    return 0;
}
```

