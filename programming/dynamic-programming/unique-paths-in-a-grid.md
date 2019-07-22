# Unique Paths in a Grid

https://www.interviewbit.com/problems/unique-paths-in-a-grid/

Given a grid of size m * n, lets assume you are starting at (1,1) and your goal is to reach (m,n). At any instance, if you are on (x,y), you can either go to (x, y + 1) or (x + 1, y).

Now consider if some obstacles are added to the grids. How many unique paths would there be?
An obstacle and empty space is marked as 1 and 0 respectively in the grid.

### Example

There is one obstacle in the middle of a 3x3 grid as illustrated below.

```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```

The total number of unique paths is 2.

Note: m and n will be at most 100.

## Hint 1

Try to come up with a DP solution.

What can be possible states and transition in matrix?

## Solution Approach

Dynamic programming FTW. 

If you look at a cell, there are atmost 2 ways to reach it. From the cell left and up. 

If the cell does not have an obstacle, then the number of ways to reach this cell would be the summation of the number of ways to reach the immediate neighbors preceding it ( left and up ).

## Solution

### Editorial
```cpp
int Solution::uniquePathsWithObstacles(vector<vector<int>> &obstacleGrid) {
    int m = obstacleGrid.size(), n = obstacleGrid[0].size();
    int uniquePaths[m + 1][n + 1];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            uniquePaths[i][j] = 0;
            if (obstacleGrid[i][j]) continue;
            if (i == 0 && j == 0) uniquePaths[i][j] = 1;
            if (i > 0) uniquePaths[i][j] += uniquePaths[i - 1][j];
            if (j > 0) uniquePaths[i][j] += uniquePaths[i][j - 1];
        }
    }
    return uniquePaths[m - 1][n - 1];
}

```

### Fastest
```cpp
int Solution::uniquePathsWithObstacles(vector<vector<int> > &A) {
    
    vector<vector<int>> dp(A.size(), vector<int>(A[0].size(), 0));
    dp[0][0] = A[0][0] ^ 1;
    for(int i = 1; i < A.size(); ++i) {
        if(!A[i][0]) dp[i][0] += dp[i-1][0];
        else dp[i][0] = 0;
    }
    for(int i = 1; i < A[0].size(); ++i) {
        if(!A[0][i]) dp[0][i] += dp[0][i-1];
        else dp[0][i] = 0;
    }
    for(int i = 1; i < A.size(); ++i)
        for(int j = 1; j < A[0].size(); ++j) {
            if(A[i][j]) continue;
            dp[i][j] += dp[i-1][j] + dp[i][j-1];
        }
    return dp[(int)A.size() - 1][(int)A[0].size() - 1];
}
```

### Lightweight
```cpp
int Solution::uniquePathsWithObstacles(vector<vector<int> > &A) {
    int m=A.size();
    int n=A[0].size();
    int i,j;
    vector<int> v(n,0);
    if(A[m-1][n-1]==1)
        return 0;
    v[n-1]=1;
    for(i=n-2;i>=0;i--){
        if(A[m-1][i]==1)
            v[i]=0;
        else v[i]=v[i+1];
    }
    for(i=m-2;i>=0;i--){
        if(A[i][n-1]==1)
            v[n-1]=0;
        for(j=n-2;j>=0;j--){
            if(A[i][j]==1)
                v[j]=0;
            else v[j]+=v[j+1];
            
        }
    }
    return v[0];
}
```

### Mine
```cpp
int paths(vector<vector<int> > A, int currX, int currY, int maxX, int maxY){
    if(currX >= maxX || currY >= maxY)
        return 0;
    else if(A[currX][currY] == 1)
        return 0;
    else if(currX == maxX-1 && currY == maxY-1)
        return 1;
    return paths(A, currX + 1, currY, maxX, maxY) + paths(A, currX, currY + 1, maxX, maxY);
}


int Solution::uniquePathsWithObstacles(vector<vector<int> > &A) {
    return paths(A, 0, 0, A.size(), A[0].size());
}
```

## Asked in

* Facebook
