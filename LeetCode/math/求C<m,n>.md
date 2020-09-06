###Problem

来源：牛客网

小Q有X首长度为A的不同的歌和Y首长度为B的不同的歌，现在小Q想用这些歌组成一个总长度正好为K的歌单，每首歌最多只能在歌单中出现一次，在不考虑歌单内歌曲的先后顺序的情况下，请问有多少种组成歌单的方法。 

##### **输入描述:**

```
每个输入包含一个测试用例。
每个测试用例的第一行包含一个整数，表示歌单的总长度K(1<=K<=1000)。
接下来的一行包含四个正整数，分别表示歌的第一种长度A(A<=10)和数量X(X<=100)以及歌的第二种长度B(B<=10)和数量Y(Y<=100)。保证A不等于B。
```

##### **输出描述:**

```
输出一个整数,表示组成歌单的方法取模。因为答案可能会很大,输出对1000000007取模的结果。
```

###Solution

**精妙之处就在于如何计算C<m,n>**  如何避免整型溢出

利用[数学归纳法](https://baike.baidu.com/item/%E6%95%B0%E5%AD%A6%E5%BD%92%E7%BA%B3%E6%B3%95)：

由**C(n,k) = C(n-1,k) + C(n-1,k-1）；**

对应于[杨辉三角](https://baike.baidu.com/item/%E6%9D%A8%E8%BE%89%E4%B8%89%E8%A7%92)：

1 0 0 0 0 0 0 0 0 0 
1 1 0 0 0 0 0 0 0 0 
1 2 1 0 0 0 0 0 0 0 
1 3 3 1 0 0 0 0 0 0 
1 4 6 4 1 0 0 0 0 0 
1 5 10 10 5 1 0 0 0 0 
1 6 15 20 15 6 1 0 0 0 
1 7 21 35 35 21 7 1 0 0 
1 8 28 56 70 56 28 8 1 0 
1 9 36 84 126 126 84 36 9 1 

```c++
#include<iostream>
using namespace std;

#define mod 1000000007
long long  arr[105][105];

void cal_c(){
    arr[0][0] = 1;
    for(int i = 1; i <= 100; ++i){
        arr[i][0] = 1;
        for(int j = 1; j <= i; ++j)
            arr[i][j] = (arr[i-1][j] + arr[i-1][j-1]) % mod;
    }
}

int main(){
    int k,a,x,b,y;
    long long ret = 0;
    cin>>k>>a>>x>>b>>y;
    cal_c();
    for(int i = 0; i <= x; ++i)
        for(int j = 0; j <= y; ++j){
            if(a*i+b*j > k) break;
            else if(a*i+b*j == k) ret = (ret + arr[x][i] * arr[y][j]) % mod;
        }
    cout<<ret;
    return 0;
}

```


