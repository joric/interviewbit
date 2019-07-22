# Coin Sum Infinite

https://www.interviewbit.com/problems/coin-sum-infinite


You are given a set of coins S. In how many ways can you make sum N assuming you have infinite amount of each coin in the set.

Note: Coins in set S will be unique. Expected space complexity of this problem is O(N).

Example :

Input: 
    S = [1, 2, 3] 
    N = 4

Return: 4

Explanation: The 4 possible ways are
{1, 1, 1, 1}
{1, 1, 2}
{2, 2}
{1, 3}  
Note that the answer can overflow. So, give us the answer % 1000007

## Hint 1

We can approach the problem by thinking of a recursive solution and then convert it to dp. We can break this into 2 parts for a particular coin:

we use this coin.
we do not use this coin.
Let us define the function as

int Rec(S[], m, N)
S[] is the set of coins.
m is current coin which you will either include or avoid.
N is the sum you want to make.

The possible states to go from Rec are
Case 1: You include the coin -> Rec(S[], m, N-S[m])
Case 2: You avoid the coin -> Rec(S[], m-1, N)

Base Cases are:

N = 0, m >= 0 Since the sum is already been made return 1.
N > 0, m = 0 Sum cannot be made as no coins left return 0.
N < 0, We have overshot the required sum return 0.
Now can you use memoization to convert this to DP?


## Hint 2

Lets say we can make the sum N - S[i] in X ways. Then if we have a coin of value S[i] we can also make a sum of N in X ways. We can memoize the number of ways in which we can make all the sums < N. This can be done by keeping a count array for all sums less than N which gives us the expected space complexity of O(N). A sum of 0 is always possible as we can pick no coins, so the base case will be count[0] = 1
Remember to avoid counting a way more than once. This can be done by choosing the coins in a particular order.

## Solution

```cpp

// editorial

class Solution {
public:
    int coinchange2(vector<int> &A, int N) {
        /* 
               num_ways[i] will be storing the number of solutions for
               sum i. We need N+1 rows as the table is constructed
               in bottom up manner using the base case (N = 0)
               */
        int num_ways[N + 1];
        int i, j, m = A.size();

        // Initialise all values with 0
        memset(num_ways, 0, sizeof(num_ways));

        // Base case (If required sum is 0)
        num_ways[0] = 1;

        // Pick all coins one by one and update the num_ways[] values
        for (i = 0; i < m; i++) {
            for (j = A[i]; j <= N; j++) {
                num_ways[j] += num_ways[j - A[i]];
            }
        }
        return num_ways[N];
    }
};


////////////////////////////////
// my solution

int Solution::coinchange2(vector<int> &coins, int sum) {
    const int mod = 1000007;
    vector<int> dp(sum+1);
    dp[0] = 1;
    for(int i=0; i<coins.size(); i++) {
        for(int j = coins[i]; j <= sum; j++) {
            dp[j] += dp[j - coins[i]];
            dp[j] %= mod;
        }
    }
    return dp[sum];
}
```