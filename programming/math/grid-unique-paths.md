# Grid Unique Paths

https://www.interviewbit.com/problems/grid-unique-paths


There is a mathematical approach to solving this problem.

Note that you have to take m + n - 2 steps in total.
You have to take (m - 1) steps going down and (n-1) steps going right.

Let 0 denote a down step and 1 denote a right step.
So we need to find out the number of strings of length m + n - 2
which have exactly m - 1 zeroes and n - 1 ones.

Essentially we need to choose the positions of '1s', and then '0s' fall into the remaining positions.

So, the answer becomes Choose(m+n-2, n - 1).
## Solution

```cpp

/* editorial */

int Solution::uniquePaths(int m, int n) {
    // m+n-2 C n-1 = (m+n-2)! / (n-1)! (m-1)! (see "n choose k" function, n! / k!(n-k)! )
    long ans = 1;
    for (int i = n; i < (m + n - 1); i++) {
        ans *= i;
        ans /= (i - n + 1);
    }
    return ans;
}

/* recursive (works!) exponential O(A*B) */

int Solution::uniquePaths(int m, int n) {
    // If either given row number is first or given column 
    // number is first 
    if (m == 1 || n == 1)
        return 1;
    // If diagonal movements are allowed then the last 
    // addition is required.
    return uniquePaths(m - 1, n) + uniquePaths(m, n - 1);
	// + numberOfPaths(m-1, n-1); 
}


/* dp */
int Solution::uniquePaths(int m, int n) {
    int dp[m][n];

    // Count of paths to reach any cell in first column is 1
    for (int i = 0; i < m; i++) 
        dp[i][0] = 1; 

    // Count of paths to reach any cell in first column is 1
    for (int j = 0; j < n; j++) 
        dp[0][j] = 1; 

    // Calculate count of paths for other cells in
    // bottom-up manner using the recursive solution
    for (int i = 1; i < m; i++) { 
        for (int j = 1; j < n; j++) 
            // By uncommenting the last part the code calculatest he total 
            // possible paths if the diagonal Movements are allowed 
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
    } 
    return dp[m - 1][n - 1];
}
```