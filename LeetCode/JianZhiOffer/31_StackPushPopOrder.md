# 栈的压入、弹出序列

### Problem

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1、2、3、4、5是某栈的压栈序列，序列4、5、3、2、1是该压栈序列对应的一个弹出序列，但4、3、5、1、2就不可能是该压栈序列的弹出序列。

### Solution

```c++
bool IsPopOrder(const int* pPush, const int* pPop, int nLength)
{
    bool isPossible = false;
    if(pPush!=nullptr && pPop!=nullptr && nLength>0)
    {
        const int* nextPush = pPush;
        const int* nextPop = pPop;
        stack<int> s;
        
        while(nextPop-nPop < nLength)
        {
            while(s.empty() || s.top() != *nextPop)
            {
                if(nextPush-nPush == nLength)
                    break;
                s.push(*nextPush);
                nextPush++;
            }
            if(s.top() != *nextPop)
                break;
            s.pop();
            nextPop++;
        }
        
        if(s.empty() && nextPop-nPop == nLength)
            isPossible = true;
    }
    return isPossible;
}
```



