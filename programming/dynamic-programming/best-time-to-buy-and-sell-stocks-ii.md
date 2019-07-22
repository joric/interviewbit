# Best Time to Buy and Sell Stocks II

https://www.interviewbit.com/problems/best-time-to-buy-and-sell-stocks-ii/

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

Example :
```
Input : [1 2 3]
Return : 2
```

## Hint 1

This problem can be solved in different ways.

Try to observe when it will be optimal to buy and sell the stock.

Or you can also try to come up with a dp solution such that what can be possible thing to do( states to go) after you purchase stock on some day.

## Hint 2

Observation based solution:

Note 1: I will never buy a stock and sell it in loss.

Note 2: If A[i] < A[i+1], I will always buy a stock on i and sell it on i+1. 

Think and try to come up with a proof on the validity of the statement.

DP based solution:

Let Dp[i] = max profit you can gain in region (i,i+1,….,n).

Then Dp[i] = max(Dp[i+1],-A[i] + max( A[j]+Dp[j] st j > i ) )

Can you come up with base cases and direction of computation now?


## Solution

### Editorial
```cpp
int Solution::maxProfit(const vector<int> &prices) {
    int total = 0, sz = prices.size();
    for (int i = 0; i < sz - 1; i++) {
        if (prices[i+1] > prices[i]) total += prices[i+1] - prices[i];
    }
    return total;
}
```

### Mine
```cpp
int Solution::maxProfit(const vector<int> &A) {
    vector<int> temp(A.size(), 0);
    int buy = A[0], flag = 0, i = 1, max_sell = INT_MIN;
    int sol = 0;
    while(i < A.size()) {
        int diff = A[i] - A[i-1];
        if(diff > 0)
            sol = sol + diff;
        i++;
    }
    return sol;
}

```

## Asked in
* Amazon
* Facebook

