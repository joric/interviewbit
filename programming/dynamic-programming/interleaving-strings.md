# Interleaving Strings

https://www.interviewbit.com/problems/interleaving-strings

Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

Example,

Given:

s1 = "aabcc",

s2 = "dbbca",

When s3 = "aadbbcbcac", return true.

When s3 = "aadbbbaccc", return false.

Return 0 / 1 ( 0 for false, 1 for true ) for this problem

# Hint 1

Let n,m be the length of s1 and s2 respectively.
You need to check if using portion upto n and portion upto m if s3 can be formed or not.
So basically last character of s3 should be something from nth postion of s1 or mth position of s2. How can you recursively simulate this?

## Solution Approach
Lets again look at the bruteforce solution for this question. 
Given the string S1, S2, S3, the first character of S3 has to match with either the first character of S1 or S2. If it matches with first character of S1, we try to see if solution is possible with remaining part of S1, all of S2, and remaining part of S3. Then we do the same thing for S2.

The pseudocode might look something like this :

```cpp
bool isInterleave(int index1, int index2, int index3) {
                // HANDLE BASE CASES HERE
    
    bool answer = false; 
    if (index1 < s1.length() && s1[index1] == s3[index3]) answer |= isInterleave(index1 + 1, index2, index3 + 1);
    if (index2 < s2.length() && s2[index2] == s3[index3]) answer |= isInterleave(index1, index2 + 1, index3 + 1);
    
    return answer;
}
```

Again, index1, index2, and index3 can only take S1.length(), S2.length() and S3.length() possibilities respectively. Can you think of a memoization solution using the observation ?

BONUS: Can you eliminate one of the state i.e. come up with something having only two arguments.

## Solution

### Editorial
```cpp
class Solution {
    private:
        string s1, s2, s3;
        short memo[101][101][101];
    public:
        bool isInterleave(int index1, int index2, int index3) {
            if (index1 == s1.length() && index2 == s2.length()) {
                return index3 == s3.length();
            }
            if (index3 >= s3.length()) return false;

            if (memo[index1][index2][index3] != -1) return memo[index1][index2][index3];
            
            bool answer = false; 
            if (index1 < s1.length() && s1[index1] == s3[index3])
                answer |= isInterleave(index1 + 1, index2, index3 + 1);

            if (index2 < s2.length() && s2[index2] == s3[index3])
                answer |= isInterleave(index1, index2 + 1, index3 + 1);
            
            return memo[index1][index2][index3] = answer;
        }

        bool isInterleave(string S1, string S2, string S3) {
            s1 = S1; 
            s2 = S2;
            s3 = S3;
            memset(memo, -1, sizeof(memo));
            if (S3.length() != S1.length() + S2.length()) return false;
            return isInterleave(0, 0, 0);
        }
};
```

### Fastest

```cpp
int Solution::isInterleave(string s1, string s2, string s3) {
    int n = s1.size(), m = s2.size(), k = s3.size();
    if(k != n + m)
        return false;
    bool dp[n+1][m+1];
    for(int i=0; i<n+1; i++)
        for(int j=0; j<m+1; j++){
            if(i==0 && j==0)
                dp[i][j] = true;
            else if (i == 0)
                dp[i][j] = (dp[i][j-1] && s2[j-1] == s3[i+j-1]);
            else if (j == 0)
                dp[i][j] = (dp[i-1][j] && s1[i-1] == s3[i+j-1]);
            else
                dp[i][j] = (dp[i-1][j] && s1[i-1] == s3[i+j-1] ) || (dp[i][j-1] && s2[j-1] == s3[i+j-1]);
        }
    return dp[n][m];
}
```

### Mine
```cpp
int iv(string& s1, string& s2, string& s3, int i1, int i2, int i3) {
    if (i1 == s1.size() && i2 == s2.size()) return i3 == s3.size();
    if (i3 >= s3.size()) return false;
    return ((i1<s1.size() && s1[i1]==s3[i3]) && iv(s1, s2, s3, i1 + 1, i2, i3 + 1))
        || ((i2<s2.size() && s2[i2]==s3[i3]) && iv(s1, s2, s3, i1, i2 + 1, i3 + 1));
}

int Solution::isInterleave(string s1, string s2, string s3) {
    return iv(s1,s2,s3,0,0,0);
}
```

## Asked in

* Google
* Yahoo

