# n个骰子的点数

### Problem

把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。

#### Solution

基于循环，没有重复计算，性能更优.

我们可以考虑用两个数组来存储骰子点数每一总数出现的次数。在一次循环中，第一个数组中的第n个数字表示骰子和为n出现的次数。那么在下一循环中，我们加上一个新的骰子。那么此时和为n的骰子出现的次数，应该等于上一次循环中骰子点数和为n-1、n-2、n-3、n-4、n-5与n-6的总和。所以我们把另一个数组的第n个数字设为前一个数组对应的第n-1、n-2、n-3、n-4、n-5与n-6之和。

```c++
int g_maxValue = 6;
void PrintProbability_Solution2(int number)
{
    if(number<1)
        return;
    int* pProbabilities[2];
    pProbabilities[0] = new int[g_maxValue*number];
    pProbabilities[1] = new int[g_maxValue*number];
    for(int i=0; i<g_maxValue*number; i++)
    {
        pProbabilities[0][i]=0;
        pProbabilities[1][i]=0;
    }
    int flag=0;
    for(int i=1; i<=g_maxValue; i++)
    {
        pProbabilities[flag][i]=1;
    }
    for(int k=2; k<=number; i++)
    {
        for(int i=0; i<k; i++)
        {
            pProbabilities[1-flag][i]=0;
        }
        for(int i=k; i<=k*g_maxValue; i++)
        {
            for(int j=1; j<=i && j<=g_maxValue; j++)
                pProbabilities[1-flag][i] += pProbabilities[flag][i-j];
        }
        flag = 1-flag;
    }
    double total = pow((double)g_maxValue, number);
    for(int i = number; i <= g_maxValue*number; ++i)
    {
        double ratio = (double)pProbabilities[flag][i] / total;
        printf("%d: %e\n", i, ratio);
    }
    delete[] pProbabilities[0];
    delete[] pProbabilities[1];
}
```





