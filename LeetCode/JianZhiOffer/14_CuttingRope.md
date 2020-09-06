# 剪绳子

### Problem

题目：给你一根长度为n绳子，请把绳子剪成m段（m、n都是整数，n>1并且m≥1）。每段的绳子的长度记为k[0]、k[1]、⋯⋯、k[m]。k[0]*k[1]*⋯*k[m]可能的最大乘积是多少？例如当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到最大的乘积18。

### Solution1

动态规划

```c++
int maxProductAfterCutting_solution1(int length)
{
   	if(length<2)
        return 0;
    if(length == 2)
        return 1;
    if(length == 3)
        return 2;
    
    int* products = new int[length+1];
    products[0] = 0;
    products[1] = 1;
    products[2] = 2;
    products[3] = 3;
    
    int max_product;
    for(int i=4; i<=length; i++)
    {
        max_product = 0
        for(int j=1; j<=i/2; j++)
        {
            int product = products[j]*products[i-j];
            if(product > max_product)
                max_product = product;
        }
        products[i] = max_product;
    }
    max_product = products[length];
    delete[] products;
    return max_product;
}
```



### Solution2

贪心算法

当n>=5时，我们尽可能多剪长度为3的绳子；当剩下的绳子长度为4时，我们把绳子剪成两段长度为2的绳子。

因为当n>=5时，3(n-3)>n 且 2(n-2)>n 且 3(n-3)>2(n-2)

```c++
int maxProductAfterCutting_solution2(int length)
{
   	if(length<2)
        return 0;
    if(length == 2)
        return 1;
    if(length == 3)
        return 2;
    
    int timesOf3 = length/3;
    if(length - timesOf3*3 ==1)
        timesOf3-=1;
    int timesOf2 = (length - timesOf3*3)/2;
    return (int)(pow(3, timesOf3) * pow(2, timesOf2));
}
```

