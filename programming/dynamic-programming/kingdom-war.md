# Kingdom War

https://www.interviewbit.com/problems/kingdom-war/

Two kingdoms are on a war right now, kingdom X and kingdom Y. As a war specialist of kingdom X, you scouted kingdom Y area.

A kingdom area is defined as a N x M grid with each cell denoting a village.
Each cell has a value which denotes the strength of each corresponding village.
The strength can also be negative, representing those warriors of your kingdom who were held hostages.

There's also another thing to be noticed.

The strength of any village on row larger than one (2<=r<=N) is stronger or equal to the strength of village which is exactly above it.
The strength of any village on column larger than one (2<=c<=M) is stronger or equal to the strength of vilage which is exactly to its left.
(stronger means having higher value as defined above).
So your task is, find the largest sum of strength that you can erase by bombing one sub-matrix in the grid.

### Input format

```
First line consists of 2 integers N and M denoting the number of rows and columns in the grid respectively.
The next N lines, consists of M integers each denoting the strength of each cell.

1 <= N <= 1500
1 <= M <= 1500
-200 <= Cell Strength <= 200
Output:

The largest sum of strength that you can get by choosing one sub-matrix.
```

### Example

```
Input:
3 3
-5 -4 -1
-3 2 4
2 5 8

Output:
19

Explanation:
Bomb the sub-matrix from (2,2) to (3,3): 2 + 4 + 5 + 8 = 19
```

## Hint 1

A simple observation is to notice that the strength on each row is larger or equal to the row above and the strength on each column is also larger or equal to the column on its left.

This means, we don't really need to check every single sub-array.

Note: Using Kadane's 2D Max Sub-Matrix Sum O(N^3) will lead to TLE

Note 2: Maximum answer might be negative.

## Solution Approach

Based on the observation in Hint 1, we can assume that the largest sub-array strength may start from any point, but will definitely end on bottom-right cell (N,M).

Therefore, we can use dynamic programming to find the sum of sub-matrix starting from the bottom-right cell (N,M) going up and left.

DP[i][j] = DP[i+1][j] + DP[i][j+1] - DP[i+1][j+1]

Find the maximum answer from DP[i][j] for each (i,j)


## Solution

### Editorial
```cpp
int Solution::solve(vector<vector<int> > &A) {
    vector <vector<int> > B = A;
    if(A.size()==0 || A[0].size()==0)   return INT_MIN;
    int r = A.size(), c = A[0].size();
    for(int i=r-2;i>=0;i--){
        for(int j=0;j<c;j++){
            B[i][j] += B[i+1][j];
        }
    }
    for(int i=0;i<r;i++){
        for(int j=c-2;j>=0;j--){
            B[i][j] += B[i][j+1];
        }
    }
    int max_sum = INT_MIN;
    for(int i=0;i<r;i++){
        for(int j=0;j<c;j++){
            if(B[i][j] > max_sum)   max_sum = B[i][j];
        }
    }
    return max_sum;
}
```

### Fastest
```cpp
int Solution::solve(vector<vector<int> > &A) {
    int mx = INT_MIN;
    int n = A.size();
    int m = A[0].size();
    for(int i=n-1;i>=0;i--){
        for(int j=m-1;j>=0;j--){
            if(i<n-1) A[i][j] += A[i+1][j];
            if(j<m-1) A[i][j] += A[i][j+1];
            if(i<n-1 && j<m-1) A[i][j] -= A[i+1][j+1];
            mx = max(mx, A[i][j]);
        }
    }
    return mx;
}
```

### Lightweight
```cpp
int Solution::solve(vector<vector<int> > &vec) {
    int row=vec.size();
    int col=vec[0].size();
    int sum=vec[row-1][col-1];
    for(int i=row-1;i>=0;i--){
        for(int j=col-1;j>=0;j--){
            if(i<row-1){
                vec[i][j]+=vec[i+1][j];
            }
            if(j<col-1){
                vec[i][j]+=vec[i][j+1];
            }
            if(i<row-1 && j<col-1){
                vec[i][j]-=vec[i+1][j+1];
            }
            sum=max(vec[i][j],sum);
        }
    }
     
    return sum;
}
```

### Mine
```cpp
int Solution::solve(vector<vector<int> > &A) {
    int ans,i,j,dn,rig,r,c,sum;
    int n=A.size();
    int m=A[0].size();
    vector<vector<int> > rsum=A;
    for(r=0;r<n;r++)
    {
        for(c=m-2;c>=0;c--)
        {
            rsum[r][c]=A[r][c]+rsum[r][c+1];
        }
    }
    int ma=A[n-1][m-1];
    int dp[n][m];
    memset(dp,0,sizeof(dp));
    for(j=m-1;j>=0;j--)
    {
        for(i=n-1;i>=0;i--)
        {
            rig=0;
            dn=0;
            if(i!=n-1)
            {
                dn=dp[i+1][j];
            }
            if(j!=m-1)
            {
                rig=rsum[i][j+1];
            }
            dp[i][j]=A[i][j]+rig+dn;
            ma=max(ma,dp[i][j]);
        }
    }
    return(ma);
}

```
