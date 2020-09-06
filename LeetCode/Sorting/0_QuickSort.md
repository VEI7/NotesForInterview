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

