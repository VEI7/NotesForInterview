# 矩阵中的路径

### Problem

 题目：请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用下划线标出）。但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

### Solution

回溯法

```c++
bool hasPath(const char* matrix, int rows, int cols, const char* str)
{
    if(matrix ==nullptr || rows<1 || cols<1 || str==nullptr)
        return false;
    bool *visited = new bool[rows*cols];
    memset(visited, 0, rows*cols);
    
    int pathLength = 0;
    for(int row=0; row<rows; row++)
    {
        for(int col=0; col<cols; col++)
        {
            if(search(matrix, rows, cols, str, row, col, pathLength, visited))
                return true;
        }
    }
    
    delete[] visited;
    return false;
}

bool search(const char* matrix, int rows, int cols, const char* str, int row, int col, int& pathLength, bool* visited)
{
    if(str[pathlength] == '\0')
        return true;
   bool hasPath = false;
   if(row>=0 && row<rows && col>=0 && col<cols && matrix[row*cols+col] == str[pathLength]
     && !visited[row*cols+col]) 
   {
       pathLength++;
       visited[row*cols+col] = true;
       hasPath = search(matrix, rows, cols, str, row, col-1, pathLength, visited)
         || search(matrix, rows, cols, str, row, col+1, pathLength, visited)
         || search(matrix, rows, cols, str, row-1, col, pathLength, visited)
         || search(matrix, rows, cols, str, row+1, col, pathLength, visited);
       
       if(!hasPath)
       {
           pathLength--;
           visited[row*cols+col] = false;
       }
           
   }
    return hasPath;
}
```

