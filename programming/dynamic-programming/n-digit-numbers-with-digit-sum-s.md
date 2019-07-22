# N digit numbers with digit sum S

https://www.interviewbit.com/problems/n-digit-numbers-with-digit-sum-s-/


Find out the number of N digit numbers, whose digits on being added equals to a given number S. Note that a valid number starts from digits 1-9 except the number 0 itself. i.e. leading zeroes are not allowed.

Since the answer can be large, output answer modulo 1000000007

```
N = 2, S = 4 
Valid numbers are {22, 31, 13, 40} 
Hence output 4.
```

## Hint 1

Try to think in terms of Recursion with two states - current digit count and current sum.

## Solution Approach

### Part 1

Lets build a recursive approach to this problem. Let rec_Count(id, sum) be the number of numbers having digit count as id and digit sum as sum. To be more clear,

rec_Count(id, sum) = SUM(rec_Count(id-1,sum-x)) where 0 <= x <= 9 && sum-x >= 0. 

Note that the above relation has not handled the leading zeroes case. How can you handle them ?

### Part 2

We can handle them by calling this rec_Count function for the first digit explicitly. i.e. we can fix the starting digits from 1-9 explicitly and then call the recursion function to handle the other digits(i.e. N - 1 digits). Finally we can add them together to get the final answer.

Gotcha : Try to think about the approach when sum is given as 0.

Now, we have the recursive solution. However, the recursive solution is too expensive because of the exponential time complexity.

A key thing to note here is that there are overlapping subproblems as many things are being calculated repeatedly in the recursive solution ? Can you use the concept of Dynamic programming to optimize the time complexity here ?

### Final solution

My recursive function only depends on id and sum variable. If ID is the max possible id, and SUM is the max possible sum, then there are only ID * SUM number of ways in which the function can be called.

We can use memoization to store those values.


## Solution

### Editorial
```cpp
int rec(vector<vector<int>> &dp, int id, int sum) {
    if (sum < 0) return 0;
    if (id == 0 && sum == 0) return 1;
    if (id == 0) return 0;

    if (dp[id][sum] != -1) return dp[id][sum];

    int ans = 0;
    for (int i = 0; i < 10; i++) {
        ans += rec(dp, id - 1, sum - i);
        ans %= 1000000007;
    }
    return dp[id][sum] = ans;
}

int Solution::solve(int A, int B) {
    int ans = 0;
    vector<vector<int>> dp;
    dp.resize(A + 1);
    for (int i = 0; i < A + 1; i++) {
        dp[i].resize(B + 1);
        for (int j = 0; j < B + 1; j++)
            dp[i][j] = -1;
    }
    for (int i = 1; i < 10; i++) {
        ans += rec(dp, A - 1, B - i);
        ans %= 1000000007;
    }
    return ans;
}

```


### Mine

```cpp
int Solution::solve(int n, int s) {
    long long int dp[n][s];
    if(s<1||s>9*n) return 0;
    
    for(int i=0;i<s;i++){
        if(i+1<=9) dp[0][i]=1;
        else dp[0][i]=0;
    }
    for(int j=0;j<n;j++){
        dp[j][0]=1;
    }
    for(int i=1;i<n;i++){
        for(int j=1;j<s;j++){
            dp[i][j]=0;
            int k=0;
            if(j>=9) k=j-9;
            for(;k<=j;k++)
             dp[i][j]=(dp[i][j]+dp[i-1][k])%1000000007;
             
        }
    }
    return dp[n-1][s-1];
}
```
