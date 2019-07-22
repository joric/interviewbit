# Distinct Subsequences

https://www.interviewbit.com/problems/distinct-subsequences/

Given two sequences S, T, count number of unique ways in sequence S, to form a subsequence that is identical to the sequence T.

 Subsequence: A subsequence of a string is a new string which is formed from the original string by deleting some (can be none ) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not). 

```
Example :

S = "rabbbit" 
T = "rabbit"
Return 3. And the formations as follows:

S1= "ra_bbit" 
S2= "rab_bit" 
S3="rabb_it"
```
"_" marks the removed character.

## Hint 1

Can you solve the problem for some prefix of first string and some prefix of second string and use it to compute the final answer?

Try to think of DP on prefixes of both strings.

## Solution Approach

As a typical way to implement a dynamic programming algorithm, we construct a matrix dp, where each cell dp[i][j] represents the number of solutions of aligning substring T[0..i] with S[0..j];

Rule 1). dp[0][j] = 1, since aligning T = "" with any substring of S would have only ONE solution which is to delete all characters in S.

Rule 2). when i > 0, dp[i][j] can be derived by two cases:

case 1). if T[i] != S[j], then the solution would be to ignore the character S[j] and align substring T[0..i] with S[0..(j-1)]. Therefore, dp[i][j] = dp[i][j-1].

case 2). if T[i] == S[j], then first we could adopt the solution in case 1), but also we could match the characters T[i] and S[j] and align the rest of them (i.e. T[0..(i-1)] and S[0..(j-1)]. As a result, dp[i][j] = dp[i][j-1] + d[i-1][j-1]

e.g. T = B, S = ABC

dp[1][2]=1: Align T'=B and S'=AB, only one solution, which is to remove character A in S'.

## Solution

### Editorial
```cpp
int Solution::numDistinct(string S, string T) {
    int m = T.length();
    int n = S.length();
    if (m > n) return 0; // impossible for subsequence
    vector<vector<int>> path(m + 1, vector<int>(n + 1, 0));
    for (int k = 0; k <= n; k++) path[0][k] = 1; // initialization

    for (int j = 1; j <= n; j++) {
        for (int i = 1; i <= m; i++) {
            path[i][j] = path[i][j - 1] + (T[i - 1] == S[j - 1] ? path[i - 1][j - 1]: 0);
        }
    }

    return path[m][n];
}
```

### Fastest
```cpp
int Solution::numDistinct(string S, string T) {
    vector<int> f(T.size()+1);
    //set the last size to 1.
    f[T.size()]=1;
    for(int i=S.size()-1; i>=0; --i){
        for(int j=0; j<T.size(); ++j){
            f[j]+=(S[i]==T[j])*f[j+1];
        }
    }
    return f[0];
}
```

### Lightweight
```cpp
int Solution::numDistinct(string S, string T) {
    vector<int> f(T.size()+1);
    //set the last size to 1.
    f[T.size()]=1;
    for(int i=S.size()-1; i>=0; --i){
        for(int j=0; j<T.size(); ++j){
            f[j]+=(S[i]==T[j])*f[j+1];
        }
    }
    return f[0];
}
```

### Mine
```cpp
int Solution::numDistinct(string S, string T) {
    int rows = T.size(), cols = S.size();
    if(rows > cols) return 0;
    vector<vector<int> > temp(rows+1, vector<int>(cols+1, 0));
    for(int i = 0; i <= cols; i++)
        temp[0][i] = 1;
    for(int i = 1; i <= rows; i++){
        for(int j = i; j <= cols; j++){
            if(S[j-1] == T[i-1])
                temp[i][j] = temp[i-1][j-1] + temp[i][j-1];
            else
                temp[i][j] = temp[i][j-1];
        }
    }
    return temp[rows][cols];
}

```

## Asked in
* Google
