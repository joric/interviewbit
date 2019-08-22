# String count

https://www.interviewbit.com/problems/string-count/

Given an integer A,
Find and return (total number of binary strings of length A, without having consecutive 1's) % 10^9 + 7.

### Input Format

The only argument given is integer A.

### Output Format

Return  (total number of binary strings of length A, without having consecutive 1's) % 10^9 + 7.

### Constraints

```
1 <= A <= 10^5
```
### For Example
```
Input 1:
    A = 2
Output 1:
    3
Explaination 1:
    "00"
    "01"
    "10"

Input 2:
    A = 3
Output 2:
    5
Explaination 2:
    "000"
    "001"
    "010"
    "100"
    "101"
```
## Hint 1

A string S of length i can be made from a string t of length i - 1 by adding '0' behind T or by adding '1' behind T if last character of T is not '1'.

## Solution Approach
```
Dp recurrence:-
DP[0][i] = DP[0][i-1] + DP[1][i-1]
DP[1][i] = DP[0][i-1]
DP[0][i] = Count of all possible binary strings of lenght i without having consecutive 1's and ending at 0
DP[1][i] = Count of all possible binary strings of length i without having consecutive 1's and ending at 1
```
## Solution
### Editorial
```cpp
int Solution::solve(int A) {
    long long mod = 1000000007;
    long long dp[2][A+1];
    dp[0][1] = 1LL;
    dp[1][1] = 1LL;
    for(int i = 2;i <= A;i++)
    {
        dp[0][i] = dp[0][i - 1] + dp[1][i - 1];
        dp[0][i] %= mod;
        dp[1][i] = dp[0][i - 1] % mod;
        dp[1][i] %= mod;
    }
    return (dp[0][A] + dp[1][A])%mod;
}
```

### Fastest
```cpp
int Solution::solve(int A) {
    int a=1,b=1,mod=1e9+7;
    for(int t=1;t<A;++t) {
        int aa = a + b;
        b = a;
        a=aa;
        if(a>=mod)a-=mod;
    }
    a+=b;
    if(a>=mod)a-=mod;
    return a;
}
```
### Lightweight
```cpp
int Solution::solve(int A) {
    int a=1,b=1,mod=1e9+7;
    for(int t=1;t<A;++t) {
        int aa = a + b;
        int bb = a;
        a=aa%mod;
        b=bb%mod;
    }
    return (a+b)%mod;
}
```

### Mine
```cpp
int Solution::solve(int n) {
    long long a[n], b[n]; 
    a[0] = b[0] = 1;
    for (int i = 1; i < n; i++) { 
        a[i] = (a[i-1] + b[i-1]) % 1000000007; 
        b[i] = a[i-1]; 
    } 
    return (a[n-1] + b[n-1]) % 1000000007;
}
```
### Python
```python
class Solution:
    def solve(self, A):
        a = b = 1
        for i in range(A):
            tmp = a
            a += b
            b = tmp
        return a % int(1e9+7)
```
