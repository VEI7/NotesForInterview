# 1143. Longest Common Subsequence

- - 

    Dr. Patel has **N** stacks of plates. Each stack contains **K** plates. Each plate has a positive *beauty value*, describing how beautiful it looks.

    Dr. Patel would like to take exactly **P** plates to use for dinner tonight. If he would like to take a plate in a stack, he must also take all of the plates above it in that stack as well.

    Help Dr. Patel pick the **P** plates that would maximize the total sum of beauty values.

    ### Input

    The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each test case begins with a line containing the three integers **N**, **K** and **P**. Then, **N** lines follow. The i-th line contains **K** integers, describing the beauty values of each stack of plates from top to bottom.

    ### Output

    For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the maximum total sum of beauty values that Dr. Patel could pick.

    ### Limits

    Time limit: 20 seconds per test set.
    Memory limit: 1GB.
    1 ≤ **T** ≤ 100.
    1 ≤ **K** ≤ 30.
    1 ≤ **P** ≤ **N** * **K**.
    The beauty values are between 1 and 100, inclusive.

    #### Test set 1

    1 ≤ **N** ≤ 3.

    #### Test set 2

    1 ≤ **N** ≤ 50.

    ### Sample

    ```
    Input
     
    2
    2 4 5
    10 10 100 30
    80 50 10 50
    3 2 3
    80 80
    15 50
    20 10
    
    Output
    Case #1: 250
    Case #2: 180
    ```



    In Sample Case #1, Dr. Patel needs to pick **P** = 5 plates:

    - He can pick the top 3 plates from the first stack (10 + 10 + 100 = 120).
    - He can pick the top 2 plates from the second stack (80 + 50 = 130) .

    In total, the sum of beauty values is 250.



    In Sample Case #2, Dr. Patel needs to pick **P** = 3 plates:

    - He can pick the top 2 plates from the first stack (80 + 80 = 160).
    - He can pick no plates from the second stack.
    - He can pick the top plate from the third stack (20).

    In total, the sum of beauty values is 180.



    **Note:** Unlike previous editions, in Kick Start 2020, all test sets are visible verdict test sets, meaning you receive instant feedback upon submission.Problem



###Solution

```c++
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
#include <set>
#include <numeric>


using namespace std;

void solve(){
    int T, K, P;
    cin >> T;
    cin >> K;
    cin >> P;
    vector<vector<int>> dp(T, vector<int>(P+1, 0));
    vector<vector<int>> A(T, vector<int>(K, 0));
    for(int i=0 ; i<T; ++i){
        vector<int> sum(K+1, 0);
        for(int j=0; j<K; j++) {
            cin >> A[i][j];
            sum[j+1] =  sum[j]+ A[i][j];
        }
        for(int j=1; j<=P; j++)
        {
            if(i>0)
            {
                for(int l=0; (l<=j && l<=K); l++)
                {
                    dp[i][j] = max(dp[i-1][j-l] + sum[l], dp[i][j]);
                }
            }
            else
                dp[i][j] = j>K ? sum[K] : sum[j];
        }
    }
    for(int i=0; i<T; i++)
    {
        for (int j = 0; j <=P; ++j) {
            cout<<dp[i][j] << ' ';
        }
        cout<<endl;
    }
    cout<<dp[T-1][P]<<endl;
}




int main(){
    ios :: sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    std::cout.precision(10);
    std::cout.setf( std::ios::fixed, std:: ios::floatfield );
    int T;
    cin >> T;
    for(int tt =1 ; tt <=T ; ++tt)
    {
        cout << "Case #" << tt << ": ";
        solve();
    }
    return 0;
}
```

