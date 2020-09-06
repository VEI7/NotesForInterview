# 二进制中1的个数

### Problem

题目：请实现一个函数，输入一个整数，输出该数二进制表示中1的个数。例如把9表示成二进制是1001，有2位是1。因此如果输入9，该函数输出2。

### Solution1

注意需要考虑负数的情况。因此将整数n向右移的方法不可取。

```c++
int numberOf1(int n)
{
   	int count=0;
    unsigned int flag=1;
    while(flag)
    {
        if(flag&n)
            count++;
        flag = flag<<1;
    }
    return count;
}
```



### Solution2

把一个整数减去1，再与原整数作与运算，会把整数最右边的1变为0。那么一个整数有多少个1，就可以进行多少次这样的操作。

```c++
int numberOf1(int n)
{
   	int count=0;
    while(n)
    {
        count++;
        n=n&(n-1);
    }
    return count;
}
```

