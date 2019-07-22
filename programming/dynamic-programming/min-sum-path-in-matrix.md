# Min Sum Path in Matrix

https://www.interviewbit.com/problems/min-sum-path-in-matrix/

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

### Note
You can only move either down or right at any point in time. 

### Example

```
Input : 

    [  1 3 2
       4 3 1
       5 6 1
    ]

Output : 8
     1 -> 3 -> 2 -> 1 -> 1
```

## Hint 1

Think Dynamic programming. How would the current minimum distance be related to the minimum distance of its neighbors ?

## Solution Approach

Let DP[i][j] store the minimum sum of numbers along the path from top left to (i,j).

Basically, DP[i][j] = A[i][j] + min(DP[i-1][j],DP[i][j-1]).

You only need to figure out the base conditions and boundary conditions now.

## Solution

### Editorial
```cpp
int Solution::minPathSum(vector<vector<int> > &grid) {
    if (grid.size() == 0) return 0;
    int m = grid.size(), n = grid[0].size();
    int minPath[m + 1][n + 1];
    for (int i = 0; i < m; i++) {
        minPath[i][0] = grid[i][0]; 
        if (i > 0) minPath[i][0] += minPath[i - 1][0];
        for (int j = 1; j < n; j++) {
            minPath[i][j] = grid[i][j] + minPath[i][j-1];
            if (i > 0) minPath[i][j] = min(minPath[i][j], grid[i][j] + minPath[i-1][j]);
        }
    }
    return minPath[m-1][n-1];
}

```

### Fastest
```cpp
int Solution::minPathSum(vector<vector<int> > &A) {
       if(A.size() == 0){
        return 0;
    }
    
    int rows = A.size();
    int cols = A[0].size();
    
    vector<vector<int> > temp(rows, vector<int>(cols));
    
    int sum = 0;
    
    for(int i = 0; i < cols; i++){
        temp[0][i] = sum + A[0][i];
        sum = temp[0][i];
    }
    
    sum = 0;
    
    for(int i = 0; i < rows; i++){
        temp[i][0] = sum + A[i][0];
        sum = temp[i][0];
    }
    
    for(int i = 1; i < rows; i++){
        for(int j = 1; j < cols; j++){
            temp[i][j] = A[i][j] + min(temp[i-1][j], temp[i][j-1]);
        }
    }
    
    return temp[rows-1][cols-1];
}

```

### Lightweight
```cpp
int Solution::minPathSum(vector<vector<int> > &A) {
    int n = A.size(), m = A[0].size();
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            int curr = INT_MAX;
            if (i-1 >= 0) {
                curr = min(curr, A[i-1][j]);
            }
            if (j-1 >= 0) {
                curr = min(curr, A[i][j-1]);
            }
            if (curr != INT_MAX) {
                A[i][j] += curr;
            }
        }
    }
    return A[n-1][m-1];
}
```

### Mine
```cpp
int Solution::minPathSum(vector<vector<int> > &A) {
    if(A.size() == 0)
        return 0;
    
    int rows = A.size();
    int cols = A[0].size();
    
    vector<vector<int> > temp(rows, vector<int>(cols));
    
    int sum = 0;
    
    for(int i = 0; i < cols; i++){
        temp[0][i] = sum + A[0][i];
        sum = temp[0][i];
    }
    
    sum = 0;
    
    for(int i = 0; i < rows; i++){
        temp[i][0] = sum + A[i][0];
        sum = temp[i][0];
    }
    
    for(int i = 1; i < rows; i++){
        for(int j = 1; j < cols; j++){
            temp[i][j] = A[i][j] + min(temp[i-1][j], temp[i][j-1]);
        }
    }
    
    return temp[rows-1][cols-1];
}

```

## Asked in
* Amazon

