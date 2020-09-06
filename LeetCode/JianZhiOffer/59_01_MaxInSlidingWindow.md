# 滑动窗口的最大值

### Problem

给定一个数组和滑动窗口的大小，请找出所有滑动窗口里的最大值。例如，如果输入数组{2, 3, 4, 2, 6, 2, 5, 1}及滑动窗口的大小3，那么一共存在6个滑动窗口，它们的最大值分别为{4, 4, 6, 6, 6, 5}，

### Solution

```c++
vector<int> maxInWindows(const vector<int>& num, unsigned int size)
{
    vector<int> maxInWindows;
    if(nums.size()>=size && size>=1)
    {
        deque<int> index;
        for(int i=0; i<size; i++)
        {
            while(!index.empty() && num[i]>num[index.back()])
                index.pop_back();
            index.push_back(i);
        }
        for(int i=size; i<num.size(); i++)
        {
            while(!index.empty() && num[i]>num[index.back()])
                index.pop_back();
            if(!index.empty() && index.front() <= (int) (i - size))
                index.pop_front();
            index.push_back(i);
        }
    }
    return maxInWindows;
}
```





