题目描述：

Hulu有一些列的视频文件，每个文件都有对应的码率，为整数。输入长度为 n 的视频码率数组 arr ，现在定义两个文件区段之间最大码率为：

 $p[i][j] = max(arr[i], arr[i+1], ... , arr[j]), 0 <= i <= j <= n-1. $

针对所有满足条件 0 <= i <= j <= n-1 的 (i,j) 对，求$ p[i][j] $的总和。

输入

第一行为 n，表示数组的长度。第二行为空格分开的 n 个码率（整数）。

输入满足：1 <= n <= 1000000， 1 <= arr[i] <= 1000000

输出

输出一个数字，即$ p[i][j] $的总和，如果总和超过 1000000007 ，则返回对1000000007取模的结果

输入样例

3

1 2 2

输出样例

11

解释：满足要求的$ p[0][0] = 1, p[0][1] = 2, p[0][2] = 2, p[1][1] = 2, p[1][2] = 2, p[2][2] = 2$. 结果为 11。



### Solution

针对每个位置的数 i , 如果它作为某些子数组的max元素，那么意味着：

    arr[x], arr[x+1], ..., arr[i], arr[i+1], ..., arr[y]

其中 max(arr[x], ..., arr[i-1]) < arr[i], max(arr[i+1], ..., arr[y]) < arr[i])

那么 i 作为子数组最大值时，一共会出现在 (i-x+1)*(y-i+1) 种子数组里。恭喜的最大值和为 (i-x+1)*(y-i+1)*arr[i]

基于此思路，针对每个 arr[i]，就只需要找出左侧和右侧大于 arr[i] 的坐标位置即可。

那么针对左侧查找，其实可以用一个递减栈来维护截止目前进入栈的值。

针对 arr[i], 如果栈顶元素小于 arr[i] 则不断弹出，直到空或者栈顶大于 arr[i]，那么该位置就是 arr[i] 的左侧大于他，且最近的第一个元素。

针对右侧也是类似。

于是时间复杂度是 O(n)，空间复杂度 O(n)



## 907.Sum of Subarray Minimums

Given an array of integers `A`, find the sum of `min(B)`, where `B` ranges over every (contiguous) subarray of `A`.

Since the answer may be large, **return the answer modulo 10^9 + 7.**

 

**Example 1:**

```
Input: [3,1,2,4]
Output: 17
Explanation: Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4]. 
Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.  Sum is 17.
```

 

**Note:**

1. `1 <= A.length <= 30000`
2. `1 <= A[i] <= 30000`

### Solution

```python
class Solution:
    def sumSubarrayMins(self, A: List[int]) -> int:
        n = len(A)
        count = [0] * n
        s = []
        for i in range(n):
            while len(s)>0 and A[i] <= A[s[-1]]:
                s.pop()
            if len(s) == 0:
                count[i] = i+1
            else:
                count[i] = i-s[-1]
            s.append(i)
        s = []
        for i in range(n-1,-1,-1):
            while len(s)>0 and A[i] < A[s[-1]]:
                s.pop()
            if len(s) == 0:
                count[i] *= (n-i)
            else:
                count[i] *= (s[-1]-i)
            s.append(i)
        res = 0
        for i in range(n):
            res += count[i]*A[i]
        return res%(10**9+7)
```

