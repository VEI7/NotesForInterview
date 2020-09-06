# 二维有序数组中的查找

### Problem

题目：在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按 照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

### Solution1

查找范围为一个矩形，如果查找的数字在矩形的右上角，则找到，退出循环；不然，如果右上角的值大于查找的数字，则查找范围删去最右侧一列（因为右上角的值为该列的最小值），如果小于，则删去最上侧一行（因为右上角的值为该行的最大值）。直到查找范围不能再缩小。同理也可比较左下角。

```c++
bool Find(int* matrix, int rows, int columns, int number)
{
    bool isFound = false;
    if(matrix!=nullptr && rows>0 &&columns >0)
    {
        int row = 0, col = columns-1;
        while(row<rows && col >=0)
        {
            if(matrix[row*columns + col] == number)
            {
                isFound = true;
                break;
            }
            else if(matrix[row*columns + col] > number)
                col--;
            else
                row++;
        }
    }
    return isFound;
    
}
```

