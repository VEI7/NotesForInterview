# Round G 2019 - Kick Start 2019

## 1.Book Reading

### Problem

Supervin is a librarian handling an ancient book with **N** pages, numbered from 1 to **N**. Since the book is too old, unfortunately **M** pages are torn out: page number **P1**, **P2**, ..., **PM**.

Today, there are **Q** lazy readers who are interested in reading the ancient book. Since they are lazy, each reader will not necessarily read all the pages. Instead, the i-th reader will only read the pages that are numbered multiples of **Ri** and not torn out. Supervin would like to know the sum of the number of pages read by each reader.

### Input

The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each test case begins with a line containing the three integers **N**, **M**, and **Q**, the number of pages in the book, the number of torn out pages in the book, and the number of readers, respectively. The second line contains **M** integers, the i-th of which is **Pi**. The third line contains **Q** integers, the i-th of which is **Ri**.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the total number of pages that will be read by all readers.

### Limits

Time limit: 40 seconds per test set.
Memory limit: 1GB.
1 ≤ **T** ≤ 100.
1 ≤ **P1** < **P2** < ... < **PM** ≤ **N**.
1 ≤ **Ri** ≤ **N**, for all i.

#### Test set 1 (Visible)

1 ≤ **M** ≤ **N** ≤ 1000.
1 ≤ **Q** ≤ 1000.

#### Test set 2 (Hidden)

1 ≤ **M** ≤ **N** ≤ 105.
1 ≤ **Q** ≤ 105.

### Sample

```
Input:
3
11 1 2
8
2 3
11 11 11
1 2 3 4 5 6 7 8 9 10 11
1 2 3 4 5 6 7 8 9 10 11
1000 6 1
4 8 15 16 23 42
1
Output:
Case #1: 7
Case #2: 0
Case #3: 994
```





In sample case #1, the first reader will read the pages numbered 2, 4, 6, and 10. Note that the page numbered 8 will not be read since it is torn out. The second reader will read the pages numbered 3, 6, and 9. Therefore, the total number of pages that will be read by all readers is 4 + 3 = 7.

In sample case #2, all pages are torn out so all readers will read 0 pages.

In sample case #3, the first reader will read all the pages other than the six given pages.

### Analysis

Let f(x) be the number of pages that are multiples of x and not torn out. To compute f(x), we can only check whether the pages x, 2x, 3x, ..., floor(**N**/x)x are torn out. Therefore, we can do this in **N**/x time.

This means that we can compute f(1), f(2), ..., f(**N**) in a total of **N**(1/1 + 1/2 + ... + 1/**N**) time. 1/1 + 1/2 + ... + 1/**N** is approximately O(log **N**) (since the n-th  harmonic number is approximately O(log **N**)), so in total f(1), f(2), ..., f(**N**) can be computed in a total of O(**N** log **N**) time.

After precomputing f(x), we can easily count the number of pages that each lazy readers will read in O(1). The running time of this solution is O(**N** log **N** + **Q**).

### Solution

```c++
#include <iostream>
#include <vector>

using namespace std;

int main() {
    int caseNum, N, M, Q;
    cin >> caseNum;
    for(int c=1; c<=caseNum; c++){
        int count = 0;
        cin >>N >>M >>Q;
        vector<int> torn(N, 0), F(N, 0);
        while(M--){
            int tp;
            cin >>tp;
            torn[tp-1] = 1;
        }
        for(int i=1; i<=N; i++){
            int j=i;
            while(j<=N){
                if(torn[j-1]==0)
                    F[i-1]++;
                j+=i;
            }
        }
        while(Q--){
            int q;
            cin >> q;
            count += F[q-1];
        }
        cout << "Case #" << c << ": "<<count<<endl;
    }
    return 0;
}
```

## 2.The Equation

### Problem

The laws of the universe can be represented by an array of **N** non-negative integers. The i-th of these integers is **Ai**.

The universe is *good* if there is a non-negative integer k such that the following equation is satisfied: (**A1** xor k) + (**A2** xor k) + ... (**AN** xor k) ≤ **M**, where xor denotes the [bitwise exclusive or](https://en.wikipedia.org/wiki/Bitwise_operation#XOR).

What is the largest value of k for which the universe is good?

### Input

The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each test case begins with a line containing the two integers **N** and **M**, the number of integers in **A** and the limit on the equation, respectively.

The second line contains **N** integers, the i-th of which is **Ai**, the i-th integer in the array.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the largest value of k for which the universe is good, or `-1` if there is no such k.

### Limits

Time limit: 15 seconds per test set.
Memory limit: 1GB.
1 ≤ **T** ≤ 100.
1 ≤ **N** ≤ 1000.

#### Test set 1 (Visible)

0 ≤ **M** ≤ 100.
0 ≤ **Ai** ≤ 100, for all i.

#### Test set 2 (Hidden)

0 ≤ M≤ 10^15

0 ≤ Ai≤ 10^15, for all i.

### Sample

```
Input:
4
3 27
8 2 4
4 45
30 0 4 11
1 0
100
6 2
5 5 1 5 1 0

Output:
Case #1: 12
Case #2: 14
Case #3: 100
Case #4: -1
```



In sample case #1, the array contains **N** = 3 integers and **M** = 27. The largest possible value of k that gives a good universe is 12 ((8 xor 12) + (2 xor 12) + (4 xor 12) = 26).

In sample case #2, the array contains **N** = 4 integers and **M** = 45. The largest possible value of k that gives a good universe is 14 ((30 xor 14) + (0 xor 14) + (4 xor 14) + (11 xor 14) = 45).

In sample case #3, the array contains **N** = 1 integer and **M** = 0. The largest possible value of k that gives a good universe is 100 (100 xor 100 = 0).

In sample case #4, there is no value of k that gives a good universe, so the answer is -1.

### Analysis

在计算异或和的时候，我们原本是计算![k](https://math.jianshu.com/math?formula=k)与每一个![A_i](https://math.jianshu.com/math?formula=A_i)的异或值，然后求和。但实际上，通过观察，可以发现，异或计算的每一位是独立的，也即，我们可以计算![k](https://math.jianshu.com/math?formula=k)的每一位与所有![A_i](https://math.jianshu.com/math?formula=A_i)这一位的异或值，最后对所有的结果求和。

这启发我们用两个数组`ones[64]`和`zeros[64]`统计所有![A_i](https://math.jianshu.com/math?formula=A_i)二进制表示中每一位上1和0的数量。然后就可以把刚才的和式重写为：

![\sum_{i=0}^{50}2^i\times(ones[i]\times(1-k[i])+zeros[i]\times k[i])](https://math.jianshu.com/math?formula=%5Csum_%7Bi%3D0%7D%5E%7B50%7D2%5Ei%5Ctimes(ones%5Bi%5D%5Ctimes(1-k%5Bi%5D)%2Bzeros%5Bi%5D%5Ctimes%20k%5Bi%5D))

其中![k](https://math.jianshu.com/math?formula=k)已经写成二进制形式。这里上限取50，是因为![2^{50}>10^{15}](https://math.jianshu.com/math?formula=2%5E%7B50%7D%3E10%5E%7B15%7D)，所以最终的结果不会超过![2^{50}](https://math.jianshu.com/math?formula=2%5E%7B50%7D)。

写成这样的形式之后，我们不难找到一条贪心策略。我们从二进制最高位开始，如果这一位上设为1后，可以异或和满足不超过![M](https://math.jianshu.com/math?formula=M)的条件，我们就把这一位设为1，因为这样得到的数一定比把这一位设为0得到的数大。

我们如何判断能不能满足条件呢？对于第![i](https://math.jianshu.com/math?formula=i)位来说，它产生的和是有最小值的，这个最小值就是![\min(ones[i],zeros[i])\times2^i](https://math.jianshu.com/math?formula=%5Cmin(ones%5Bi%5D%2Czeros%5Bi%5D)%5Ctimes2%5Ei)。所以我们可以预先计算出从第0位到第50位的最小值，并求出累积和`min_val[50]`。

得到这一累积和后，我们就可以很容易地判断某一位上取1之后，后面的位置是否存在一种取法，使得总异或和不超过![M](https://math.jianshu.com/math?formula=M)。只要这一条件能够满足，我们就取1。否则检查取0时能否满足，如果能满足，就取0。如果取0也不行，说明无解。

### Solution:

```c++
#include <iostream>
#include <vector>

typedef long long ll;
using namespace std;

int main() {
    int caseNum, N;
    ll M;
    cin >> caseNum;
    for(int c=1; c<=caseNum; c++){
        int count = 0;
        cin >>N >>M;
        vector<int> zeros(50, 0), ones(50, 0);
        while(N--){
            ll ni;
            cin >>ni;
            for(int i=0; i<50; i++){
                if(ni>>i & 1)
                    ones[i]++;
                else
                    zeros[i]++;
            }
        }

        vector<ll> min_val(50,0);
        ll sum =0;
        for(int i=0; i<50; i++){
            ll num = (ll)1 << i;
            sum += (ll)min(ones[i], zeros[i]) *num;
            min_val[i] = sum;
        }
        ll k=0;
        sum =0;
        for(int i=49; i>=0; i--){
            ll num = (ll)1 << i;
            ll one = num * zeros[i];
            ll zero = num * ones[i];
            if(sum + one + (i>0 ? min_val[i-1] : 0) <= M){
                k |= (ll)1<<i;
                sum += one;
            }
            else if(sum + zero + (i>0 ? min_val[i-1] : 0) <= M){
                sum += zero;
            }
            else{
                k = -1;
                break;
            }
        }
        cout << "Case #" << c << ": "<<k<<endl;
    }
    return 0;
}
```

##3.Shift

### Problem

Aninda and Boon-Nam are security guards at a small art museum. Their job consists of **N** shifts. During each shift, at least one of the two guards must work.

The two guards have different preferences for each shift. For the i-th shift, Aninda will gain **Ai** happiness points if he works, while Boon-Nam will gain **Bi** happiness points if she works.

The two guards will be happy if both of them receive at least **H** happiness points. How many different assignments of shifts are there where the guards will be happy?

Two assignments are considered different if there is a shift where Aninda works in one assignment but not in the other, or there is a shift where Boon-Nam works in one assignment but not in the other.

### Input

The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each test case begins with a line containing the two integers **N** and **H**, the number of shifts and the minimum happiness points required, respectively. The second line contains **N** integers. The i-th of these integers is **Ai**, the amount of happiness points Aninda gets if he works during the i-th shift. The third line contains **N** integers. The i-th of these integers is **Bi**, the amount of happiness points Boon-Nam gets if she works during the i-th shift.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the number of different assignments of shifts where the guards will be happy.

### Limits

Time limit: 40 seconds per test set.
Memory limit: 1GB.
1 ≤ **T** ≤ 100.
0 ≤ **H** ≤ 109.
0 ≤ **Ai** ≤ 109.
0 ≤ **Bi** ≤ 109.

#### Test set 1 (Visible)

1 ≤ **N** ≤ 12.

#### Test set 2 (Hidden)

1 ≤ **N** ≤ 20.

### Sample

```
Input:
2
2 3
1 2
3 3
2 5
2 2
10 30

Output:
Case #1: 3
Case #2: 0
```

In Sample Case #1, there are **N** = 2 shifts and **H** = 3. There are three possible ways for both Aninda and Boon-Nam to be happy:

- Only Aninda works on the first shift, while both Aninda and Boon-Nam work on the second shift.
- Aninda and Boon-Nam work on the first shift, while only Aninda works on the second shift.
- Both security guards work on both shifts.



In Sample Case #2, there are **N** = 2 shifts and **H** = 5. It is impossible for both Aninda and Boon-Nam to be happy, so the answer is 0.

### Solution1:剪枝

这题有一个很容易看出来的穷举方法。因为要求每一天至少有一个人值班，所以每一天实际上有三种情况：A值班，B值班，或者A、B都值班。那么我们就可以穷举每一天的情况，然后检查是否满足条件。这样的时间复杂度是![O(3^n)](https://math.jianshu.com/math?formula=O(3%5En))，对于小数据集![N\leq 12](https://math.jianshu.com/math?formula=N%5Cleq%2012)没有问题，但大数据集![N\leq 20](https://math.jianshu.com/math?formula=N%5Cleq%2020)，而![3^{20}](https://math.jianshu.com/math?formula=3%5E%7B20%7D)约等于![3.4\times10^9](https://math.jianshu.com/math?formula=3.4%5Ctimes10%5E9)，显然会超时。



剪枝是搜索类问题中的常用策略。这一题可以怎么剪枝呢？

1. 如果穷举到某一天时，发现即使后面所有天都给A排班，A的快乐值也不够，或者所有天都给B排班，B的快乐值也不够，那么这条分支就不用继续向下搜索了。
2. 如果穷举到某一天时，发现已经满足A和B的快乐值都不少于H，那么剩下来的![k](https://math.jianshu.com/math?formula=k)天一共![3^k](https://math.jianshu.com/math?formula=3%5Ek)种情况都能满足要求。所以直接累加到总的方法数中，不再继续搜索。

由于本题数据比较弱，使用这两个剪枝条件就可以通过了。

```c++
#include <iostream>
#include <vector>
#include <algorithm>

typedef long long ll;
using namespace std;

ll res,h;
vector<ll> three;

void dfs(vector<ll> a, vector<ll> b, vector<ll> ca, vector<ll> cb, ll asum, ll bsum, int n){
    if(n==0){
        return;
    }
    for(int i=1; i<=3; i++){
        ll as = asum, bs = bsum;
        if(i&1)
            as += a[n-1];
        if(i&2)
            bs += b[n-1];
        if(as+ca[n-1]<h || bs+cb[n-1]<h)
            continue;
        if(as>=h && bs>=h){
            res += three[n-1];
            continue;
        }
        dfs(a,b,ca,cb,as,bs,n-1);
    }
}

int main() {
    int caseNum, n;
    ll t=1;
    for(int i=0; i<21; i++){
        three.push_back(t);
        t *= (ll)3;
    }

    cin >> caseNum;
    for(int c=1; c<=caseNum; c++){
        cin>>n>>h;
        vector<ll> a(n),b(n),ca(n+1),cb(n+1);
        for (int i = 0; i < n; ++i){
            cin>>a[i];
            ca[i+1]=ca[i]+a[i];
        }
        for (int i = 0; i < n; ++i){
            cin>>b[i];
            cb[i+1] = cb[i]+b[i];
        }
        if(ca[n] < h || cb[n] < h)
            cout<< "Case #" << c << ": "<<0<<endl;
        else{
            res=0;
            dfs(a,b,ca,cb,0,0,n);
            cout<< "Case #" << c << ": "<<res<<endl;
        }

    }
    return 0;
}
```

### Solution2

朴素穷举的最大问题是底数是3，如果我们能把底数降到2，![2^{20}\simeq10^6](https://math.jianshu.com/math?formula=2%5E%7B20%7D%5Csimeq10%5E6)就是一个可以处理的数据规模了。

很容易想到把A和B分开处理。计算A能够满足条件的排法，再计算B能够满足条件的排法。问题是，如何去掉不合法的情况，也即存在没有人站岗的情况的排法？

我们用一个![N](https://math.jianshu.com/math?formula=N)位的二进制数`state`表示A每天的站岗情况，1表示A这天站岗，0表示A这天不站岗。我们先假设A、B不同时站岗。那么A不站岗的时候，B就要站岗。我们可以计算出每一个`state`对应的B得到的快乐值，如果不少于![H](https://math.jianshu.com/math?formula=H)，就记为`f[state]=1`。

接下来，我们需要派生出包含A、B都站岗的情形。对于一个已经满足A的`state`，我们保持A的状态不变，然后对于B的状态将其中的1替换为0（也即B在已经有A值班的时候也值班），就可以得到所有满足A和B的快乐值的`(A，B)`状态组。这样可以保证每天至少有一个人站岗。

比如说，`state`是`1001`，且满足A的快乐值不少于![H](https://math.jianshu.com/math?formula=H)，那么就可以从`1001`这个`state`，得到了`(1001, 1001)`、`(1001, 0001)`、`(1001, 1000)`、`(1001, 0000)`这几个状态。第一个数表示A的排班表，1站岗0不站岗；第二个数表示B的排班表，0站岗1不站岗。

为了不重复不遗漏，在派生过程中，我们需要一位一位进行（可以参考代码中这部分内外循环的顺序）。

这样，我们就得到了所有满足B的快乐值不小于![H](https://math.jianshu.com/math?formula=H)的方法，然后，我们再检查是否满足A的快乐值不小于![H](https://math.jianshu.com/math?formula=H)即可。

```c++
#include <iostream>
#include <vector>
#include <algorithm>

typedef long long ll;
using namespace std;


int main() {
    int caseNum, n;
    ll h;
    cin >> caseNum;
    for(int c=1; c<=caseNum; c++){
        ll res=0;
        cin>>n>>h;
        ll states = (1<<n);
        vector<ll> a(n), b(n), f(states);
        for (int i = 0; i < n; ++i)
            cin>>a[i];
        for (int i = 0; i < n; ++i)
            cin>>b[i];
        // 检查B的快乐值是否被满足。1:A站岗，0:B站岗
        for (int i = 0; i < states; ++i) {
            ll sum=0;
            for(int j=0; j<n; j++){
                if(!(i &(1<<j)))
                    sum += b[j];
            }
            if(sum>=h)
                f[i]++;
        }
        for(int j=0; j<n; j++)
            for (int i = 0; i < states; ++i)
                if(i&(1<<j))
                    f[i] += f[i^(1<<j)];
        for (int i = 0; i < states; ++i) {
            ll sum=0;
            for(int j=0; j<n; j++){
                if(i &(1<<j))
                    sum += a[j];
            }
            if(sum>=h)
                res+=f[i];

        }
        cout<< "Case #" << c << ": "<<res<<endl;
    }
    return 0;
}
```





