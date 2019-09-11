# Matrix Chain Multiplication

https://www.interviewbit.com/problems/matrix-chain-multiplication/

Given an array of integers A representing chain of 2-D matices such that the dimensions of ith matrix is
A[i-1] x A[i].

Find the most efficient way to multiply these matrices together. The problem is not actually to perform the multiplications,
but merely to decide in which order to perform the multiplications.

Return the minimum number of multiplications needed to multiply the chain.

### Input Format

The only argument given is the integer array A.

### Output Format

Return the minimum number of multiplications needed to multiply the chain.

### Constraints

1 <= length of the array <= 1000

1 <= A[i] <= 100

### For Example

```
Input 1:
    A = [40, 20, 30, 10, 30]
Output 1:
    26000
    Explanation 1:
        Dimensions of A1 = 40 x 20
        Dimensions of A2 = 20 x 30
        Dimensions of A3 = 30 x 10
        Dimensions of A4 = 10 x 30
        First, multiply A2 and A3 ,cost = 20*30*10 = 6000
        Second, multilpy A1 and (Matrix obtained after multilying A2 and A3) =  40 * 20 * 10 = 8000
        Third, multiply (Matrix obtained after multiplying A1, A2 and A3) and A4 =  40 * 10 * 30 = 12000
        Total Cost = 12000 + 8000 + 6000 =26000

Input 2:
    A = [10, 20, 30] 
Output 2:
    6000
```

## Solution Approach

Matrix Multiplication is associative, if we have four matrices W,X,Y and Z.

WXYZ = W(XY)Z = (WX)((YZ) = (W)(X(YZ))

The Problem reduces to place parenthesis such that the cost is minimum.

For example, if the given chain is of 4 matrices. let the chain be WXYZ,
then there are 3 ways to place first set of parenthesis outer side: (W)(XYZ), (WX)(YZ) and (WXY)(Z).

So when we place a set of parenthesis, we divide the problem into subproblems of smaller size.

Therefore, the problem has optimal substructure property and can be easily solved using recursion.

But This will lead to exponential time complexity.

You can reduce the unnecessary function because there is overlapping Subproblem property in its recursive implemenation.

Use dynamic programming to reduce unnecessary calls.

Refer Complete Solution for Implementaion.

## Solution

### Editorial

```cpp
int Solution::solve(vector<int> &A) {
    if(A.size()-1 == 2) return A[0]*A[1]*A[2];
    vector<vector<int>> dp(A.size(),vector<int>(A.size(),INT_MAX));
    for (int i = 1; i < A.size(); i++) 
        dp[i][i] = 0; 
    for(int l=2;l<A.size();l++){
        for(int i=0;i<A.size()-l+1;i++){
            int j= (i+l)-1;
            for(int k=i;k<j;k++){
                dp[i][j] = min(dp[i][j],dp[i][k]+dp[k+1][j] + A[i-1]*A[k]*A[j]);
            }
        }
    }
    return dp[1][A.size()-1];
}
```
### Fastest
```cpp
int Solution::solve(vector<int> &A) {
int n=A.size();

int m[n][n];

for(int i=0;i<n;i++)
    m[i][i]=0;
    
for(int L=2;L<n;L++){
    for(int i=1;i<n-L+1;i++){
        int j=i+L-1;
        m[i][j]=INT_MAX;
        for(int k=i;k<j;k++){
            m[i][j]=min(m[i][j],m[i][k]+m[k+1][j]+A[i-1]*A[k]*A[j]);
        }
    }
}

return m[1][n-1];

}
```

### Lightweight
```cpp
int Solution::solve(vector<int> &p) {
    int n = p.size();
    
        int m[n][n]; 

    int i, j, k, L, q; 

    /* m[i,j] = Minimum number of scalar multiplications needed 
    to compute the matrix A[i]A[i+1]...A[j] = A[i..j] where 
    dimension of A[i] is p[i-1] x p[i] */

    // cost is zero when multiplying one matrix. 
    for (i=1; i<n; i++) 
        m[i][i] = 0; 

    // L is chain length. 
    for (L=2; L<n; L++) 
    { 
        for (i=1; i<n-L+1; i++) 
        { 
            j = i+L-1; 
            m[i][j] = INT_MAX; 
            for (k=i; k<=j-1; k++) 
            { 
                // q = cost/scalar multiplications 
                q = m[i][k] + m[k+1][j] + p[i-1]*p[k]*p[j]; 
                if (q < m[i][j]) 
                    m[i][j] = q; 
            } 
        } 
    } 

    return m[1][n-1]; 
}
```
