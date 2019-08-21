# Grid Unique Paths II

https://www.interviewbit.com/problems/grid-unique-paths-ii/


A matrix of integers A of size N x M represents a grid consisting of 0 and 1.
A robot is located at the top-left corner of this grid.
The robot can only move either down or right at any point in time.
The robot is trying to reach the bottom-right corner of the grid.

if A[i][j] == 1 this represents a obstacle at position (i, j). You cannot walk over obstacles and 
cannot enter a cell containing obstacle.

if A[i][j] == 0 this represents a empty at position (i, j). You can free freely over empty cells and enter them.

Your task is to find how many unique paths are there from the top-left corner to the bottom right corner 
if only down move or right move is allowed.
Since the number of paths can be very large your task is to find the number of unique paths modulo (109+7).

### Input Format

The only argument given is the matrix A.

### Output Format

Return the number of unique paths modulo (10^9+7).

### Constraints

```
1 <= N, M <= 500
0 <= A[i] <= 1
```

### For Example
```
Input 1:
    A = [   [0, 0, 0]
            [0, 1, 0]
            [0, 0, 0] ]
Output 1:
    2
    Explanation 1:
        There are only two paths possible:
            1. Right -> Right -> Down -> Down
            2. Down -> Down -> Right -> Right

Input 2:
    A : [   [0, 1]
            [0, 0]  ]
Output 2:
    1
```

## Hint 1
Number of ways to reach any cell(i,j) = number of ways to reach any cell(i-1,j) + number of ways to reach any cell(i-1,j).
Find base condition and try to implement using dynamic programming.

## Solution Approach

Number of ways to reach any cell(i,j) = number of ways to reach any cell(i-1,j) + number of ways to reach any cell(i-1,j).
let dp[i][j] = number of ways to reach cell(i,j) from top left corner.

Base condition
```
if A[0][0] = 0
    dp[0][0] = 1
else 
    dp[0][0] = 0
```

if the top left cell is blocked there are 0 ways to reach bottom right corner.
for the cells in first row, they can only be reached by cell from left in the first row.
Similary for the cells in first column, they can only be reached by cell from up in the first column.
for the rest of cells dp moves forward like this:

```
if(A[i][j] == 0)
    dp[i][j] = dp[i-1][j]+dp[i][j-1]
else
    dp[i][j]=0
```

dp[N-1][M-1] contains the number of unique ways to reach bottom right corner from top left corner.

## Solution
### Editorial
```cpp
#define mod 1000000007
int Solution::solve(vector<vector<int> > &A) {
    int n = A.size(),m = A[0].size();
   if(n == 0) return 0;
   if(A[0][0] == 1) return 0;
   vector<vector<int>> dp(n,vector<int>(m,0));
   dp[0][0] = 1;
   for(int i=1;i<n && A[i][0] != 1;i++) dp[i][0] = 1;
   for(int i=1;i<m && A[0][i] != 1;i++) dp[0][i] = 1;
   for(int i=1;i<n;i++){
       for(int j=1;j<m;j++){
           if(A[i][j] == 0) dp[i][j] = (dp[i-1][j]*1ll + dp[i][j-1]*1ll)%mod;
       }
   }
   return dp[n-1][m-1];
}
```
### Fastest
```cpp
#define mod 1000000007
int Solution::solve(vector<vector<int> > &A) {
    int n = A.size(),m = A[0].size();
   if(n == 0) return 0;
   if(A[0][0] == 1) return 0;
   vector<vector<int>> dp(n,vector<int>(m,0));
   dp[0][0] = 1;
   for(int i=1;i<n && A[i][0] != 1;i++) dp[i][0] = 1;
   for(int i=1;i<m && A[0][i] != 1;i++) dp[0][i] = 1;
   for(int i=1;i<n;i++){
       for(int j=1;j<m;j++){
           if(A[i][j] == 0) dp[i][j] = (dp[i-1][j]*1ll + dp[i][j-1]*1ll)%mod;
       }
   }
   return dp[n-1][m-1];
}
```

## Lightweight (mine)
```cpp
int Solution::solve(vector<vector<int>> &dp) {
    int n = dp.size(), m = dp[0].size();
    dp[0][0] = 1;

    for (int i = 1; i < n; i++)
        dp[i][0] = !dp[i][0] && dp[i - 1][0] ? 1 : 0;
        
    for (int i = 1; i < m; i++)
        dp[0][i] = !dp[0][i] && dp[0][i - 1] ? 1 : 0;
        
    for (int i = 1; i < n; i++)
        for (int j = 1; j < m; j++)
            dp[i][j] = (dp[i][j] ? 0 : dp[i - 1][j] + dp[i][j - 1]) % 1000000007;
            
    return dp[n - 1][m - 1];
}
```
