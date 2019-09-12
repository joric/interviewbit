# Find subsequence

https://www.interviewbit.com/problems/find-subsequence/

Given two strings A and B, find if A is a subsequence of B.

Return "YES" if A is a subsequence of B, else return "NO".

### Input Format

The first argument given is the string A.

The second argument given is the string B.

### Output Format

Return "YES" if A is a subsequence of B, else return "NO". **Constraints**

1 <= lenght(A), length(B) <= 100000

'a' <= A[i], B[i] <= 'z'

### For Example
```
Input 1:
    A = "bit"
    B = "dfbkjijgbbiihbmmt"
Output 1:
    YES

Input 2:
    A = "apple"
    B = "appel"
Output 2:
    "NO"
```

## Solution Approach

1. Traverse both A and B from left to right. If we find a matching character, we move ahead in both strings. Otherwise we move ahead only in B.
2. if A finishes first return "YES".
3. Else return "NO";

## Solution
### Editorial
```cpp
string Solution::solve(string A, string B) {
    /*vector<vector<int>> dp(A.length()+1,vector<int>(B.length()+1,0));
    for(int i=1;i<=A.length();i++){
        for(int j= 1 ;j<=B.length();j++){
            if(A[i-1] == B[j-1]) dp[i][j] = dp[i-1][j-1]+1;
            else dp[i][j] = max(dp[i-1][j] ,dp[i][j-1]);
        }
    }
    if( dp[A.length()][B.length()] == A.length()) return "YES";
    return "NO";*/
    int l  = A.length()-1;
    for(int i=B.length()-1;i>=0;i--){
        if(B[i] == A[l]){
            l--;
            if(l == -1) return "YES";
        }
    }
    if(l == -1) return "YES";
    else return "NO";
}
```
### Fastest
```cpp
string Solution::solve(string A, string B) {
    int m=A.size();
    int n=B.size();
    int j=0,f=1;
    for(int i=0;i<n;i++)
    {
        if(A[j]==B[i])
        {
        //    cout<<A[j];
            j++;
            if(j==m)
            {
               f=0;
               break;
            }
        }
    }
    if(f==0)
    {
        return "YES";
    }
    else
    {
        return "NO";
    }
}
```
### Lightweight
```cpp
string Solution::solve(string A, string B) {
    int m = A.size(),n = B.size();
    int i=0,j=0;
    while(i<m and j<n){
        if(A[i]==B[j]){
            i++;j++;
        }
        else j++;
    }
    if(i<m) return "NO";
    else return "YES";
}
```

### Mine
```cpp
string Solution::solve(string A, string B) {
    int pos = 0;
    for (char c:A) if (!(pos = B.find(c, pos)+1)) return "NO";
    return "YES";
}
```
