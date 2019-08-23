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
    int n = s.length();
    vector<vector<int>> dp(n+1, vector<int>(n+1, 0));
    for (int i = n-1; i>=0; i--) {
        dp[i][i] = 1;
        for (int j = i+1; j<n; j++)
            dp[i][j] = s[i]==s[j] ? dp[i+1][j-1]+2 : max(dp[i+1][j], dp[i][j-1]);
    }
    return dp[0][n - 1];
}
```
