# Best Time to Buy and Sell Stocks III

https://www.interviewbit.com/problems/best-time-to-buy-and-sell-stocks-iii/

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most two transactions.

### Note

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### Example

```
Input : [1 2 1 2]
Output : 2

Explanation:
  Day 1 : Buy
  Day 2 : Sell
  Day 3 : Buy
  Day 4 : Sell
```

## Hint 1

Think DP.

What are the essential things you need to keep in your DP states?

You will need to store maximum number of transactions you can do in any suffix/prefix of array.

## Solution Approach

What if you construct your DP space as : 

f[k, ii] represents the max profit up until prices[ii] (Note: NOT ending with prices[ii]) using at most k transactions

How would you fill in values in f[k, ii] and how would the DP relations look like.

## Solution

### Editorial
```cpp
int Solution::maxProfit(const vector<int> &prices) {
    // f[k, ii] represents the max profit up until prices[ii] (Note: NOT ending with prices[ii]) using at most k transactions. 
    // f[k, ii] = max(f[k, ii-1], prices[ii] - prices[jj] + f[k-1, jj]) { jj in range of [0, ii-1] }
    //          = max(f[k, ii-1], prices[ii] + max(f[k-1, jj] - prices[jj]))
    // f[0, ii] = 0; 0 times transation makes 0 profit
    // f[k, 0] = 0; if there is only one price data point you can't make any money no matter how many times you can trade
    if (prices.size() <= 1) return 0;
    int K = 2; // number of max transation allowed
    int maxProf = 0;
    vector<vector<int> > f(K+1, vector<int>(prices.size(), 0));
    for (int kk = 1; kk <= K; kk++) {
        int tmpMax = f[kk-1][0] - prices[0];
        for (int ii = 1; ii < prices.size(); ii++) {
            f[kk][ii] = max(f[kk][ii-1], prices[ii] + tmpMax);
            tmpMax = max(tmpMax, f[kk-1][ii] - prices[ii]);
            maxProf = max(f[kk][ii], maxProf);
        }
    }
     
    return maxProf;
    
}

```

### Mine
```cpp
int Solution::maxProfit(const vector<int> &prices) {
    int buy1 = INT_MIN, buy2 = INT_MIN;
    int sell1 = 0, sell2 = 0;
    for (auto a: prices) {
        sell2 = max(sell2, a + buy2);
        buy2 = max(buy2, sell1 - a);
        sell1 = max(sell1, a + buy1);
        buy1 = max(buy1, -a);
    }
    return sell2;
}

```

## Asked in
* Amazon
* Facebook

