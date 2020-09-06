# 最小的k个数

### Problem

输入n个整数，找出其中最小的k个数。例如输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

### Solution

使用STL中的multiset来实现最大堆，插入删除最大堆可达到O(logk)的复杂度。最终的时间复杂度为O(nlogk)

```c++
typedef multiset<int, greater<int> > intSet;
typedef multiset<int, greater<int> >::iterator setIterator;

void GetLeastNumbers(const vector<int>& data, intSet& leastNumbers, int k)
{
    leastNumbers.clear();
    if(k<1 || data.size()<k)
        return;
    vector<int>::const_iterator iter=data.begin();
    for(;iter<data.end(); iter++)
    {
        if(leastNumbers.size()<k)
            leastNumbers.insert(*iter);
        else
        {
            if(*iter < *(leastNumbers.begin())
               {
                   leastNumbers.erase(leastNumbers.begin());
                   leastNumbers.insert(*iter); 
               }
        }
    }
}
```



