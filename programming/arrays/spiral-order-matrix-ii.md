# Spiral Order Matrix II

https://www.interviewbit.com/problems/spiral-order-matrix-ii/

Given an integer n, generate a square matrix filled with elements from 1 to n2 in spiral order.

Example:

Given n = 3,

You should return the following matrix:

```
[
  [ 1, 2, 3 ],
  [ 8, 9, 4 ],
  [ 7, 6, 5 ]
]
```

## Solution

```cpp
vector<vector<int> > Solution::generateMatrix(int A) {

    int n = A;
    vector<vector<int>> result_matrix(n, vector<int>(n));
 
    // Normal Case
    int rowStart = 0;
    int rowEnd = n-1;
    int colStart = 0;
    int colEnd = n-1;
    int num = 1;
 
    while (rowStart <= rowEnd && colStart <= colEnd) 
    {
        for (int i = colStart; i <= colEnd; i ++) // 1. horizonal, left to right
        {
            result_matrix[rowStart][i] = num ++; 
        }
        rowStart ++;
 
        for (int i = rowStart; i <= rowEnd; i ++) // 2. vertical, top to bottom
        {
            result_matrix[i][colEnd] = num ++; 
        }
        colEnd --;
 
        for (int i = colEnd; i >= colStart; i --)  // 3. horizonal, right to left 
        {
            if (rowStart <= rowEnd)
                result_matrix[rowEnd][i] = num ++;
        }
        rowEnd --;
 
        for (int i = rowEnd; i >= rowStart; i --)  // 4. vertical, bottom to  top 
        {
            if (colStart <= colEnd)
                    result_matrix[i][colStart] = num ++;
        }
        colStart ++;
    }
        
    return result_matrix;
}
```