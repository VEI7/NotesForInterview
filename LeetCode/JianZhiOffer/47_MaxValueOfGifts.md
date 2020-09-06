# 礼物的最大价值

### Problem

在一个m×n的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向左或者向下移动一格直到到达棋盘的右下角。给定一个棋盘及其上面的礼物，请计算你最多能拿到多少价值的礼物？

### Solution

```c++
int getMaxValue_solution1(const int* values, int rows, int cols)
{
    if(values == nullptr || rows<=0 || cols<=0)
        return 0;
    int** maxValues = new int*[rows];
    for(int i=0; i<rows; i++)
        maxValues[i] = new int[cols];
    
    for(int i=0; i<rows; i++)
        for(int j=0; j<cols; j++)
        {
            int left=0, up=0;
            if(i>0)
                up = maxValues[i-1][j];
            if(j>0)
                left = maxValues[i][j-1];
            maxValues[i][j] = max(up,left)+values[i*cols+j];
        }
    
    int maxValue = maxValues[rows-1][cols-1];
    for(int i=0; i<rows; i++)
        delete[] maxValues[i];
    delete[] maxvalues;
    return maxValue;
}
```





