# 数据流中的中位数

### Problem

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

### Solution

```c++
template<typename T> class DynamicArray
{
    private:
    vector<T> min;//最小堆
    vector<T> max;//最大堆
    
    public:
    	void Insert(T num)
        {
            if((min.size()+max.size())&1==0)
            {
                if(max.size()>0 && num < max[0])
                {
                    max.push_back(num);
                    push_heap(max.begin(), max.end(), less<T>());
                    num = max[0];
                    pop_heap(max.begin(), max.end(), less<T>());
                    max.pop_back();
                }
                min.push_back(num);
                push_heap(min.begin(), min.end(), greater<T>());
            }
            else
            {
                if(min.size() > 0 && min[0] < num)
                {
                    min.push_back(num);
                    push_heap(min.begin(), min.end(), greater<T>());

                    num = min[0];

                    pop_heap(min.begin(), min.end(), greater<T>());
                    min.pop_back();
                }

                max.push_back(num);
                push_heap(max.begin(), max.end(), less<T>());
            }
        }
    	T GetMedian()
        {
            int size = min.size() + max.size();
            if(size == 0)
                throw exception("No numbers are available");

            T median = 0;
            if((size & 1) == 1)
                median = min[0];
            else
                median = (min[0] + max[0]) / 2;

            return median;
        }
}
```



