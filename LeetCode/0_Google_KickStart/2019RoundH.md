# 

# Round H 2019 - Kick Start 2019

## 1.H-index

### Problem

It is important for researchers to write many high quality academic papers. Jorge has recently discovered a way to measure how impactful a researcher's papers are: the [H-index](https://en.wikipedia.org/wiki/H-index).

The *H-index score* of a researcher is the largest integer h such that the researcher has h papers with at least h citations each.

Jorge has written **N** papers in his lifetime. The i-th paper has **Ai** citations. The number of citations that each paper has will never change after it is written. Please help Jorge determine his H-index score after each paper he wrote.

### Input

The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each test case begins with a line containing **N**, the number of papers Jorge wrote.

The second line contains **N** integers. The i-th integer is **Ai**, the number of citations the i-th paper has.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is a space-separated list of integers. The i-th integer is the H-index score after Jorge wrote his i-th paper.

### Limits

Time limit: 50 seconds per test set.
Memory limit: 1GB.
1 ≤ **T** ≤ 100.
1 ≤ **Ai** ≤ 105.

#### Test set 1 (Visible)

1 ≤ **N** ≤ 1000.

#### Test set 2 (Hidden)

1 ≤ **N** ≤ 105.

### Sample

```
Input:
2
3
5 1 2
6
1 3 3 2 2 15

Output:
Case #1: 1 1 2
Case #2: 1 1 2 2 2 3
```



In Sample Case #1, Jorge wrote N= 3 papers.

- After the 1st paper, Jorge's H-index score is 1, since he has 1 paper with at least 1 citation.
- After the 2nd paper, Jorge's H-index score is still 1.
- After the 3rd paper, Jorge's H-index score is 2, since he has 2 papers with at least 2 citations (the 1st and 3rd papers).

In Sample Case #2, Jorge wrote N= 6 papers.

- After the 1st paper, Jorge's H-index score is 1, since he has 1 paper with at least 1 citation.
- After the 2nd paper, Jorge's H-index score is still 1.
- After the 3rd paper, Jorge's H-index score is 2, since he has 2 papers with at least 2 citations (the 2nd and 3rd papers).
- After the 4th paper, Jorge's H-index score is still 2.
- After the 5th paper, Jorge's H-index score is still 2.
- After the 6th paper, Jorge's H-index score is 3, since he has 3 papers with at least 3 citations (the 2nd, 3rd and 6th papers).

### Analysis

We initialize the H-index with 0, and after adding each paper, we need to update the current H-index. We can use a minimum priority queue data structure to store the citation numbers. As we go through each of the papers, we keep updating the current H-index. We also keep updating the priority queue so that it only contains numbers greater than the current H-index and remove the rest. For each new paper, we can follow these steps

1. If the current citation number is bigger than the current answer, add it in the priority queue
2. Remove all the citation numbers from the priority queue which is not greater than the current answer
3. If the size of priority queue is not less than the current answer + 1, increment the answer by 1

A single operation of priority queue takes O(log**N**) time, and a number is added and removed at most once. This leads to a total time complexity of O(**N** × log**N**) per test case, which is sufficient for test set 2.

### Solution

```c++
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

typedef long long ll;
using namespace std;

int main() {
    int caseNum, n;

    cin >> caseNum;
    for(int c=1; c<=caseNum; c++){
        cin >>n;
        vector<int> res(n, 0);
        priority_queue<int,vector<int>,greater<int> > q;
        int a, h=0;
        for(int i = 0; i < n; ++i){
            cin >>a;
            if(a>h)
                q.push(a);
            while(q.top()<=h)
                q.pop();
            if(q.size()>h)
                h++;
            res[i] = h;
        }

        cout << "Case #" << c << ": ";
        for(int j=0; j<n-1; j++){
            cout<<res[j]<<" ";
        }
        cout<<res[n-1]<<endl;
    }
    return 0;
}
```

## 2.Diagonal Puzzle

### Problem

Kibur has made a new puzzle for you to solve! The puzzle consists of an **N** by **N** grid of squares. Each square is either black or white. The goal of the puzzle is to make all the squares black in as few moves as possible.

In a single move, you may choose any diagonal of squares and flip the color of every square on that diagonal (black becomes white and white becomes black). For example, the 10 possible diagonals for a 3 by 3 grid are shown below.

```
/..      ./.      ../      ...      ...
...      /..      ./.      ../      ...
...      ...      /..      ./.      ../


...      ...      \..      .\.      ..\
...      \..      .\.      ..\      ...
\..      .\.      ..\      ...      ...
```



Given the initial configuration of the board, what is the fewest moves needed to make all the squares black? You are guaranteed that it is possible to make all the squares black.

### Input

The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each test case begins with a line containing the integer **N**, the size of the grid. Then, **N** lines follow, each containing **N** characters that describe the initial configuration of the grid. The c-th character on the r-th line is the character `.` (ASCII number 46) if the square in the r-th row and c-th column is initially white. Otherwise, it is `#` (ASCII number 35), indicating that it is black.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the fewest moves needed to make all the squares black.

### Limits

Time limit: 20 seconds per test set.
Memory limit: 1GB.
1 ≤ **T** ≤ 100.
You are guaranteed that it is possible to make all the squares black.

#### Test set 1 (Visible)

2 ≤ **N** ≤ 8.

#### Test set 2 (Hidden)

2 ≤ **N** ≤ 100.

### Sample

```
Input:
3
3
..#
#.#
#..
5
.####
#.###
##.##
###.#
#####
2
##
##

Output:
Case #1: 3
Case #2: 2
Case #3: 0
```



In sample case #1, the fewest moves needed is 3, as shown below:

```
..#    ..#    .##    ###
#.# -> ..# -> #.# -> ###
#..    ##.    ##.    ###
```

In sample case #2, the fewest moves needed is 2, as shown below:

```
.####    #####    #####
#.###    #####    #####
##.## -> ##### -> #####
###.#    #####    #####
#####    ####.    #####
```

In sample case #3, all the squares in the grid are already black, so the answer is 0.

### Analysis

One interesting observation is that if we decide whether we would flip the two largest diagonals or not, we can pick all other diagonals to flip deterministically. There are four choices for flipping/not flipping the largest two diagonals. And for each of these choices, we can go through all other diagonals in the grid. For each of these other diagonals, we can check if the cell where it intersected with one of the largest diagonals is white. If it's white, we need to flip this diagonal, otherwise not. For each of those four combinations of the largest diagonal flips, if after all the flips(both largest two diagonals and others), the final state has all black cells, then we have a possible answer, otherwise not. We take the minimum flips of the four possible choices.

There are 4 choices for the flipping of the two largest diagonals. And for each of the choices, the flipping of all other diagonals, and finally checking for all black cells can be done in O(**N**2) time, which leads to a total time complexity of O(**N**2), which is sufficient for test set 2.

An alternative solution is to convert this problem into a variant of [2-coloring](https://en.wikipedia.org/wiki/Graph_coloring#Vertex_coloring). Each of the cells in the grid is shared by two diagonals. If a cell is white, either one of them needs to be flipped to make this cell black. Otherwise, none of these two diagonals needs to be flipped. For this problem, we can consider each diagonal as a vertex of a graph, and and the squares in the grid are the edges between the two diagonals that affect that square. If the square is initially black, then the endpoints of the corresponding edge must be colored the same, else they must be colored differently. By color, we mean chosen to be flipped or not flipped. So we should color in such a way that all conditions for all edges in the graph are satisfied and we have the minimum number of flips possible. This can be done with a DFS for each component of the graph.

There are total O(**N**2) edges in the graph. Building the graph and solving it can be done in O(|Edges|). This leads to a time complexity of O(**N**), which is sufficient for test set 2.

### Solution:

```c++
  
#include<bits/stdc++.h>
#include<bits/extc++.h>
#define ms(x,v) memset((x),v,sizeof(x))
#define INF 0x3f3f3f3f
using namespace std;
// using namespace __gnu_pbds;

const int MAXN = 100+10;
typedef long long LL;
typedef pair<int,bool> PIB;



void dfs(int u,bool flip,int &cnt,vector<bool>& col,vector<bool>&vis,const vector<vector<PIB>> & G){
    if(vis[u]){
        if(col[u] != flip)
            cnt = INF;
        return;
    }   
    vis[u] = true;
    col[u] = flip;
    if(flip)cnt ++ ;
    for(const auto & e : G[u])
        dfs(e.first,flip ^ e.second,cnt,col,vis,G);
}

void solve(){
    int n;
    cin >> n;
    const int offset = 2 *n -2 + n;
    const int MAX_V = 4 *n - 2;
    vector<vector<PIB> > G(MAX_V);
    for(int i=0 ; i<n ; ++i){
        string s;
        cin >> s;
        for(int j=0 ; j<n ; ++j)
        {
            int x = i + j;
            int y = i-j + offset;
            bool v = (s[j] == '.');
            G[x].push_back(make_pair(y,v));
            // cout  << x << " " << y << " "<< v << "\n";
            G[y].push_back(make_pair(x,v));
        }
    }

    vector<bool> vis1(MAX_V,false),vis2(MAX_V,false),col(MAX_V,false);
    int ans =0;

    for(int i=0 ; i< MAX_V ; ++i){
        if(vis1[i])
            continue;
        int cnt1 = 0,cnt2 = 0;
        dfs(i,0,cnt1,col,vis1,G);
        dfs(i,1,cnt2,col,vis2,G);

        ans += min(cnt1,cnt2);
    }
    cout << ans << '\n';
}


int main(){
    ios :: sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    std::cout.precision(10);
    std::cout.setf( std::ios::fixed, std:: ios::floatfield );
    int T;
    cin >> T;
    // cout << T << '\n';
    for(int tt =1 ; tt <=T ; ++tt)
    {
        cout << "Case #" << tt << ": ";
        solve();
    }
    return 0;
}
```

##3.Elevanagram

### Problem

It is a well known fact that a number is divisible by 11 if and only if the alternating sum of its digits is equal to 0 modulo 11. For example, 8174958 is a multiple of 11, since 8 - 1 + 7 - 4 + 9 - 5 + 8 = 22.

Given a number that consists of digits from 1-9, can you rearrange the digits to create a number that is divisible by 11?

Since the number might be quite large, you are given integers **A1**, **A2**, ..., **A9**. There are **Ai** digits i in the number, for all i.

### Input

The first line of the input gives the number of test cases, **T**. **T** lines follow. Each line contains the nine integers **A1**, **A2**, ..., **A9**.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is `YES` if the digits can be rearranged to create a multiple of 11, and `NO` otherwise.

### Limits

Time limit: 20 seconds per test set.
Memory limit: 1GB.
1 ≤ **T** ≤ 100.
1 ≤ **A1** + **A2** + ... + **A9**.

#### Test set 1 (Visible)

0 ≤ **Ai** ≤ 20, for all i.

#### Test set 2 (Hidden)

0 ≤ **Ai** ≤ 109, for all i.

### Sample

```
Input:
6
0 0 2 0 0 1 0 0 0
0 0 0 0 0 0 0 0 12
0 0 0 0 2 0 1 1 0
3 1 1 1 0 0 0 0 0
3 0 0 0 0 0 3 0 2
0 0 0 0 0 0 0 1 0

Case #1: YES
Case #2: YES
Case #3: NO
Case #4: YES
Case #5: YES
Case #6: NO
```

- In Sample Case #1, the digits are `336`, which can be rearranged to `363`. This is a multiple of 11 since 3 - 6 + 3 = 0.
- In Sample Case #2, the digits are `999999999999`, which is already a multiple of 11, since 9 - 9 + 9 - 9 + ... - 9 = 0.
- In Sample Case #3, the digits are `5578`, which cannot be rearranged to form a multiple of 11.
- In Sample Case #4, the digits are `111234`, which can be rearranged to `142131`. This is a multiple of 11 since 1 - 4 + 2 - 1 + 3 - 1 = 0.
- In Sample Case #5, the digits are `11177799`, which can be rearranged to `19191777`. This is a multiple of 11 since 1 - 9 + 1 - 9 + 1 - 7 + 7 - 7 = -22 (which is 0 modulo 11).
- In Sample Case #6, the only digit is `8`, which cannot be rearranged to form a multiple of 11.

### Solution:动态规划

设数字 i选择的正数为 $k_i$, 则他对答案的贡献为 $\sum (k_i - (A_i-k_i)) i \ %11$。 因此设 表$dp[i][j]$示前 i个数中 最终 mod 11为 j的正数的个数情况，表示为一个set。

优化：

We can prove the two following conclusions:

1. If there are at least two numbers of **Ai** ≥ 10, then the solution must exist.

This is very easy to prove. We can only adjust these two digits from -5 to 5, and each adjustment will result in a different remain value of modulo 11. Thus, we will get 0 finally.

2. If there are at least three numbers of **Ai** ≥ 6, then the solution must exist.

To prove this, we can prove that:

```
  For any 1 ≤ i < j < k ≤ 9, 0 ≤ r ≤ 10, the following equations:
  (1) (i * x1 + j * x2 + k * x3) % 11 = r
  (2) x1 + x2 + x3 = 0
  (3) -3 ≤ xi ≤ 3
  will have a valid solution.
```

This can be proved by iterating all possible situations, where the total number is 9**C**3 * 11 * 7 * 7 = 45276, quite small.



```c++
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
#include <set>
#include <numeric>

#define ms(x,v) memset((x),v,sizeof(x))
#define INF 0x3f3f3f3f
using namespace std;

const int MAXN = 100+10;
typedef long long LL;


void solve(){
    vector<int> A(9);
    int sum = 0, ten=0, six=0;
    for(int i=0 ; i<A.size() ; ++i){
        cin >> A[i];
        sum += A[i];
        if(A[i]>=10){
            ten ++;
            six ++;
        }
        else if(A[i]>=6)
            six++;
    }
    if(ten >= 2 || six>=3){
        cout << "YES\n";
        return;
    }

    vector<vector<set<int> > > dp(10,vector<set<int> >(11));
    bool flag = true;
    for(int i=0 ; i<A.size() ; ++i){
        for(int k = 0 ; k<=A[i]; ++k){
            int d = ((2*k - A[i]) * (i+1)) % 11;
            d = (d >= 0) ? d :d+11;
            if(flag)
                dp[i+1][d].insert(k);
            else
                for(int j=0 ; j<11 ; ++j){
                    int v = (d+j) % 11;
                    if(!dp[i][j].empty())
                        for(auto e : dp[i][j]){
                            dp[i+1][v].insert((e+k));
                        }
                }
        }
        if(A[i] > 0) flag = false;
    }

    int x = 0;

    if(dp[9][x].count((sum)/2) )
        cout << "YES\n";
    else cout << "NO\n";
}


int main(){
    ios :: sync_with_stdio(0);
    cin.tie(0);
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





