# 圆圈中最后剩下的数字

### Problem

0, 1, ⋯, n-1这n个数字排成一个圆圈，从数字0开始每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。

### Solution1

用环形链表模拟圆圈，效率低

```c++
int LastRemaining_Solution1(unsigned int n, unsigned int m)
{
    if(n < 1 || m < 1)
        return -1;

    unsigned int i = 0;

    list<int> numbers;
    for(i = 0; i < n; ++ i)
        numbers.push_back(i);

    list<int>::iterator current = numbers.begin();
    while(numbers.size() > 1)
    {
        for(int i = 1; i < m; ++ i)
        {
            current ++;
            if(current == numbers.end())
                current = numbers.begin();
        }

        list<int>::iterator next = ++ current;
        if(next == numbers.end())
            next = numbers.begin();

        -- current;
        numbers.erase(current);
        current = next;
    }

    return *(current);
}
```

### Solution2

推导递归公式

首先我们定义一个关于n和m的方程f(n,m)，表示在n个数字中删去第m个数字最后剩下的数字。在这n个数字中，第一个被删除的数字是(m-1)%n，为了表示简单，我们记为k=(m-1)%n。这时变成了以k+1开始的一个环，即k+1,... n,0,...,k-1，我们把这剩下的n-1个数进行映射，映射到0-n-2。我们把映射定义为p，p(x)=(x-k-1)%n，其对应的逆映射为p‘(x)=(x+k+1)%n。f(n,m)=p'(f(n-1,m))=(f(n-1,m)+k+1)%n=(f(n-1,m)+m)%n。于是我们可以得到如下的递归式：

f(n,m) =0    n=1

f(n,m) =(f(n-1,m)+m)%n    n>1

```c++
int LastRemaining_Solution1(unsigned int n, unsigned int m)
{
    if(n < 1 || m < 1)
        return -1;
    int last=0;
    for(int i=2; i<=n; i++)
        last = (last+m)%i;
    return last;
}
```





