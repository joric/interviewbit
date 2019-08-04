# Best Time to Buy and Sell Stock atmost B times

https://www.interviewbit.com/problems/best-time-to-buy-and-sell-stock-atmost-b-times

Given an array of integers A of size N in which ith element is the price of the stock on day i,

You can complete atmost B transactions.

Find the maximum profit you can achieve.

NOTE: You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### Input Format

The First argument given is the integer array A.
The Second argument is integer B.

### Output Format

Return the maximum profit you can achieve by doing atmost B transactions.

### Constraints
```
1 <= N <= 500
0 <= A[i] <= 10^6
0 <= B <= 10^9
```
### For Example

```
Input 1:
    A = [2, 4, 1]
    B = 2
Output 1:
    2
    Explanation 1:
        Buy on day 1 (price = 2) and sell on day 2 (price = 4), 
        profit = 4 - 2 = 2

Input 2:
    A = [3, 2, 6, 5, 0, 3]
    B = 2
Output 2:
    7
    Explanation 1:
        Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6 - 2 = 4.
        Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3 - 0 = 3.
```

## Hint 1

Think DP.

What are the essential things you need to keep in your DP states?

A transaction is defined as one buy + sell.

dp[i][j] = maximum profit from most i transactions using prices[0..j]

## Solution Approach

dp[i][j] = maximum profit from most i transactions using prices[0..j]

A transaction is defined as one buy + sell.

Now on day j, we have two options:

Do nothing (or buy) which doesn't change the acquired profit:
`dp[i][j] = dp[i][j-1]`

Sell the stock: In order to sell the stock, you must've bought it on a day `t=[0..j-1]`.

Maximum profit that can be attained is `t:0->j-1 max(prices[j]-prices[t]+dp[i-1][t-1])`

where `prices[j]-prices[t]` is the profit from buying on day t and selling on day j. 

`dp[i-1][t-1]` is the maximum profit that can be made with at most i-1 transactions
with prices prices `[0..t-1]`.

Time complexity of this approach is `O(n^2 * k)`.

In order to reduce it to `O(n*k)`, we must find

`t:0->j-1 max(prices[j]-prices[t]+dp[i-1][t-1])` this expression in constant time.

If you see carefully,

`t:0->j-1 max(prices[j]-prices[t]+dp[i-1][t-1])` is same as

`prices[j] + t:0->j-1 max(dp[i-1][t-1]-prices[t])`

Second part of the below expression 

`MaxDiff = t:0->j-1 max(dp[i-1][t-1]-prices[t])`


can be included in the dp loop by keeping track of the maximum value till j-1.



## Solution
### Editorial
```cpp
int Solution::solve(vector<int> &prices, int K) {
    int n = prices.size();
    if (n <= 1)     return 0;
    if (K == 0)     return 0;
    
    if (K >= n/2)       //(eg part 2 ques : unlimited k)
    {
        int profit = 0;
        for (int i = 1; i < n; i++)
        {
            profit += max(0, prices[i]-prices[i-1]);
        }
        return profit;
    }
    
    vector<int> buy (K, -prices[0]);
    vector<int> sell (K, 0);
    
    for (int i = 1; i < n; i++)
    {
        buy[0] = max(buy[0], -prices[i]);
        sell[0] = max(sell[0], buy[0] + prices[i]);
        for (int k = 1; k < K; k++)
        {
            buy[k] = max(buy[k], sell[k-1] - prices[i]);
            sell[k] = max(sell[k], buy[k] + prices[i]);
        }
    }
    
    return sell[K-1];
    
    
}
```

### Fastest
```cpp
  int maxProfit(vector<int>& prices,int k) {
    if(k==0 || prices.size() == 0)
        return 0;
    int max_profit = 0;
    if(k >= prices.size()/2) {
        for(int i = 1;i < prices.size();i++) {
            max_profit += max(prices[i] - prices[i-1], 0);
        }
        return max_profit;
    }
    vector<vector<int>> dp(k+1, vector<int>(prices.size() + 1, 0));

    for(int i=1;i<=k;i++) {
        int maxDiff = -prices[0];
        for(int j = 2;j <= prices.size();j++) {
            dp[i][j] = max(dp[i][j-1], prices[j-1] + maxDiff);
            maxDiff = max(maxDiff, dp[i-1][j] - prices[j-1]);
        }
    }
    return dp[k][prices.size()];
}

int Solution::solve(vector<int> &A, int B) {
    return maxProfit(A,B);
}
```

### Lightweight
```cpp
int maxProfit(vector<int>& prices,int k) {
    if(k==0 || prices.size() == 0)
        return 0;
    int max_profit = 0;
    if(k >= prices.size()/2) {
        for(int i = 1;i < prices.size();i++) {
            max_profit += max(prices[i] - prices[i-1], 0);
        }
        return max_profit;
    }
    vector<vector<int>> dp(k+1, vector<int>(prices.size() + 1, 0));

    for(int i=1;i<=k;i++) {
        int maxDiff = -prices[0];
        for(int j = 2;j <= prices.size();j++) {
            dp[i][j] = max(dp[i][j-1], prices[j-1] + maxDiff);
            maxDiff = max(maxDiff, dp[i-1][j] - prices[j-1]);
        }
    }
    return dp[k][prices.size()];
}

int Solution::solve(vector<int> &A, int B) {
    return maxProfit(A,B);
}
```

### Another solution
```cpp
int maxProfit(vector<int>& prices,int k) {
    if(k==0 || prices.size() == 0)
        return 0;
    int max_profit = 0;
    if(k >= prices.size()/2) {
        for(int i = 1;i < prices.size();i++) {
            max_profit += max(prices[i] - prices[i-1], 0);
        }
        return max_profit;
    }
    vector<vector<int>> dp(k+1, vector<int>(prices.size() + 1, 0));

    for(int i=1;i<=k;i++) {
        int maxDiff = -prices[0];
        for(int j = 2;j <= prices.size();j++) {
            dp[i][j] = max(dp[i][j-1], prices[j-1] + maxDiff);
            maxDiff = max(maxDiff, dp[i-1][j] - prices[j-1]);
        }
    }
    return dp[k][prices.size()];
}

int Solution::solve(vector<int> &A, int B) {
    return maxProfit(A,B);
}
```

