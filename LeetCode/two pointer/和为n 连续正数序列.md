### Problem

输入一个正数n，输出所有和为n连续正数序列。

例如，输入15，由于1+2+3+4+5=4+5+6=7+8=15，所以输出3 个连续序列1-5、4-6 和7-8。

### Solution

滑动窗口，用两个数small和big分别表示序列的最小值和最大值。首先把small初始化为 1，big初始化为 2。如果从small到big的序列的和大于n的话，我们向右移动small，相当于从序列中去掉较小的数字。如果从small到big的序列的和小于n的话，我们向右移动big，相当于向序列中添加big的下一个数字。一直到small等于(1+n)/2，因为序列至少要有两个数字。

```python
def get_all_seq(n):
    left = 1
    right = 2
    seq_sum = 3
    while left < (n+1)/2:
        if seq_sum == n:
            print(left, right)
            seq_sum -= left
            left += 1
        elif seq_sum < n:
            right += 1
            seq_sum += right
        else:
            seq_sum -= left
            left += 1
```

