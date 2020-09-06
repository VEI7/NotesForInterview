# 丑数

### Problem

我们把只包含因子2、3和5的数称作丑数（Ugly Number）。求按从小到大的顺序的第1500个丑数。例如6、8都是丑数，但14不是，因为它包含因子7。习惯上我们把1当做第一个丑数。

### Solution

采用空间换时间的方法，存储一个数组来记录所有找到的丑数，下一个丑数一定是一个小一些的丑数的2、3、5的倍数。

```c++
int Min(int number1, int number2, int number3)
{
    int min = (number1<number2) ? number1 :number2;
    return (min<number3) ? min :number3;
}

int GetUglyNumber_Solution2(int index)
{
    if(index<=0)
        return 0;
    int *uglyNumbers = new int[index];
    uglyNumbers[0]=1;
    int nextUglyIndex = 1;
    
    int *ugly2 = uglyNumbers;
    int *ugly3 = uglyNumbers;
    int *ugly5 = uglyNumbers;
    
    while(nextUglyIndex < index)
    {
        int minUgly = Min(*ugly2 * 2, *ugly3 * 3, *ugly5 * 5);
        uglyNumbers[nextUglyIndex] = minUgly;
        nextUglyIndex++;
        
        while(ugly2 * 2 <= minUgly)
            ugly2++;
        while(ugly3 * 3 <= minUgly)
            ugly3++;
        while(ugly5 * 5 <= minUgly)
            ugly5++;
    }
    int ugly = uglyNumbers[nextUglyIndex-1];
    delete[] uglyNumbers;
    return ugly;
}
```





