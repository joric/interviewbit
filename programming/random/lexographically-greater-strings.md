# Lexographically Greater Strings

https://www.interviewbit.com/problems/lexographically-greater-strings/

Given a string C consisting of lowercase English alphabets of size A.
For each string D of length n,its beauty relative to C is defined as the
number of pairs of indexes i, j (1 <= i <= j <= n),
such that substring D[i..j] is lexicographically larger than substring C[i..j].

Return the count of strings D, such that their beauty relative to C equals exactly B.

Since this count can be very large you are required to return count modulo (109+7).

Note: Your solution will run on multiple test cases, Make sure to clear global variables every time.

### Input Format

```
The First argument is an integer A.
The Second argument is an integer B.
The Third argument is String C.
```

### Output Format

Return the count of strings D, such that their beauty relative to C equals exactly B modulo (10^9+7).

### Constraints

```
1 <= A <= 2000
0 <= B <= 2000
```

### For Example

```
Input 1:
    A = 2 
    B = 2
    C = "yz"
Output 1:
    26

Input 2:
    A = 2 
    B = 3
    C = "yx"
Output 2:
    3
```

## Solution
### Editorial
```cpp
const long long MOD =1000000007;

long long dp[3][2005][2005];

int doit(int n,int k,string &s){
    for(int i=0; i<3;++i)
        for(int j=0; j<=n; ++j)
            for(int pp=0; pp<=k; ++pp)
                    dp[i][j][pp]=0;
    dp[1][0][0] = 1;
    for(int i=1;i<=n;i++){
        for(int j=0;j<=k;j++){
            long long p_sum = 0;
            for(int k=0;k<3;k++)
                p_sum += dp[k][i-1][j];
            p_sum %= MOD;
            dp[0][i][j]=p_sum;
            dp[1][i][j]=(s[i-1]-'a')*p_sum%MOD;
            dp[2][i][j]=0;
            long long add = n-i+1;
            for(int prev=i;prev>0;prev--){
                if(add>j) break;
                dp[2][i][j]+=(26-(s[i-1]-'a'+1))*(dp[1][prev-1][j-add]+dp[2][prev-1][j-add])%MOD;
                if(dp[2][i][j]>=MOD)
                    dp[2][i][j]-=MOD;
                add += n-i+1;
            }
        }
    }

    long long ans = 0;
    for(int i=0;i<3;i++)
        ans += dp[i][n][k];
    ans %= MOD;
    return (int)(ans);
}

int Solution::solve(int A, int B, string C) {
    return doit(A,B,C);
}
```

## References
* https://leetcode.com/problems/minimum-cost-to-hire-k-workers/

