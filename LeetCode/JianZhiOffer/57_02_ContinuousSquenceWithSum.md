# 为s的连续正数序列

### Problem

输入一个正数s，打印出所有和为s的连续正数序列（至少含有两个数）。例如输入15，由于1+2+3+4+5=4+5+6=7+8=15，所以结果打印出3个连续序列1〜5、4〜6和7〜8。

### Solution

```c++
void FindContinuousSequence(int sum)
{
    if(sum<3)
        return;
    int small=1;
    int big=2;
    int middle = (1+sum)/2;
    int curSum = 3;
    while(small<middle)
    {
        if(curSum==sum)
        {
            PrintContinuousSequence(small,big);
        }
        while(curSum>sum && small<middle)
        {
            curSum-=small;
            small++;
            if(curSum==sum)
                PrintContinuousSequence(small,big);
        }
        big++;
        curSum+=big;
    }
}

void PrintContinuousSequence(int small, int big)
{
    for(int i = small; i <= big; ++ i)
        printf("%d ", i);

    printf("\n");
}
```





