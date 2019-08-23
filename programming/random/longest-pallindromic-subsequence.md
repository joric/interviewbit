# Longest Pallindromic Subsequence

https://www.interviewbit.com/problems/longest-pallindromic-subsequence

Given a strings A. Find the common pallindromic sequence ( A sequence which does not need to be contiguous and is a pallindrome),
which is common in itself

You need to return the length of such longest common subsequence.

### NOTE

Your code will be run on multiple test cases (<=10). Try to come up with an optimised solution.

### CONSTRAINTS

1 <= Length of A, B <= 10^3 + 5

### EXAMPLE INPUT

A : "bebeeed"

### EXAMPLE OUTPUT

4

### EXPLANATION

The longest common pallindromic subsequence is "eeee", which has a length of 4

## Solution
### Mine
```cpp
int Solution::solve(string s) {
    int n = s.size();
    string rev = s;
    reverse(s.begin(), s.end());
    vector<vector<int>> dp(n+1, vector<int>(n+1));
    for(int i=0; i<=n; i++)
        for(int j=0; j<=n; j++)
            if (!i || !j) dp[i][j] = 0;
            else if (s[i-1] == rev[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
            else if (dp[i-1][j] >= dp[i][j-1]) dp[i][j] = dp[i-1][j];
            else dp[i][j] = dp[i][j-1];
    return dp[n][n];
}
```
