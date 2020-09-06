# Round G - Kick Start 2019. The Equation

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



